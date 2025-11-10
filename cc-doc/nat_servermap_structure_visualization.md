# NAT Servermap 数据结构图形化说明

## 生成时间
2025-11-10

## 概述
为NAT服务器映射（Servermap）数据结构创建了两个DOT图形文件，用于可视化展示其哈希表结构和节点关系。

## 生成的文件

### 1. ccc-nat_servermap_structure.dot
**用途**: 展示完整的数据结构层次关系

**主要内容**:
- 全局哈希表指针 (g_nsm_by_address_hash_table_inter/ext)
- 哈希表结构 (ip_nat_servermap_hash_table_inter/ext)
- 哈希桶链结构 (ip_nat_servermap_hash_chain)
- ServerMap节点详细结构 (ip_nat_servermap_node)
- Internal Tuple列表 (ip_nat_servermap_internal_tuple)
- 定时器结构 (ip_nat_servermapping_timeout)

**特点**:
- 采用自顶向下布局 (rankdir=TB)
- 使用子图集群分组相关结构
- 使用不同颜色区分 internal/external 哈希表
- 详细展示每个结构体的字段

### 2. ccc-nat_servermap_hash_chain.dot
**用途**: 展示哈希表的链式结构和节点挂载关系

**主要内容**:
- 双哈希表并列展示 (Internal hash 和 External hash)
- 哈希桶数组的可视化
- ServerMap节点如何同时挂载在两个哈希表上
- Internal tuple list 的链接关系
- 哈希冲突时的链式结构

**特点**:
- 采用从左到右布局 (rankdir=LR)
- 模仿 ip_conntrack_hash 的展示风格
- 清晰显示同一节点的双重索引机制
- 包含具体的示例数据 (IP地址和端口)

## 数据结构关键点

### 双哈希表设计
```
Internal Hash: 按 (i_saddr, i_sport) 索引
External Hash: 按 (e_addr, e_port) 索引
```

每个 `ip_nat_servermap_node` 通过两个 `hlist_node` 字段同时链接到两个哈希表:
- `nsm_node_by_internal_address_hash` -> Internal哈希表
- `nsm_node_by_ext_address_hash` -> External哈希表

### 引用计数机制
- `refcnt`: 记录有多少个 internal_tuple 引用该 servermap_node
- 初始值为1，每增加一个复用连接时递增
- 用于判断何时可以释放 servermap_node

### Internal Tuple List
- 记录所有复用该映射的原始出站连接
- 使用链表结构 (list_head)
- 包含完整的连接五元组信息

### 定时器机制
- `snat_ct_del_time`: 记录出站连接销毁的时间戳
- `map_init_timer_flag`: 标记是否已设置延迟释放定时器
- 允许在连接老化后延迟释放 servermap 节点

## 使用方法

### 1. 生成图片 (使用 Graphviz)
```bash
# PNG 格式
dot -Tpng ccc-nat_servermap_structure.dot -o nat_servermap_structure.png
dot -Tpng ccc-nat_servermap_hash_chain.dot -o nat_servermap_hash_chain.png

# SVG 格式 (可缩放)
dot -Tsvg ccc-nat_servermap_structure.dot -o nat_servermap_structure.svg
dot -Tsvg ccc-nat_servermap_hash_chain.dot -o nat_servermap_hash_chain.svg

# PDF 格式
dot -Tpdf ccc-nat_servermap_structure.dot -o nat_servermap_structure.pdf
dot -Tpdf ccc-nat_servermap_hash_chain.dot -o nat_servermap_hash_chain.pdf
```

### 2. 在线查看
可以使用以下在线工具直接查看DOT文件:
- https://dreampuf.github.io/GraphvizOnline/
- http://www.webgraphviz.com/
- https://edotor.net/

## 设计说明

### 哈希表大小
```c
NAT_SERVERMAP_HASH_SIZE  // 哈希桶数量
```

### 哈希桶结构
```c
struct ip_nat_servermap_hash_chain {
    struct hlist_head nsm_hlist;    // 链表头
    tb_rte_rwlock_t nsmh_lock;      // 读写锁保护
    tb_rte_atomic32_t count;        // 桶中节点计数
}
```

### ServerMap 节点核心字段
```c
struct ip_nat_servermap_node {
    // 双向索引
    struct hlist_node nsm_node_by_ext_address_hash;
    struct hlist_node nsm_node_by_internal_address_hash;

    // 原始连接列表
    struct list_head internal_tuple_list;

    // 地址映射信息
    unsigned int e_addr;    // 外部地址
    u16 e_port;             // 外部端口
    unsigned int i_saddr;   // 内部地址
    u16 i_sport;            // 内部端口

    // 管理信息
    tb_rte_atomic32_t refcnt;           // 引用计数
    tb_rte_atomic32_t inter_tuple_count; // tuple数量
    long snat_ct_del_time;              // 删除时间戳
    u8 map_init_timer_flag;             // 定时器标志
}
```

## 工作流程

### 1. 新建映射
1. 收到第一个出站连接 (i_saddr:i_sport -> dst_ip:dst_port)
2. 分配外部地址和端口 (e_addr:e_port)
3. 创建 `ip_nat_servermap_node`，refcnt=1
4. 计算两个哈希键值 (i_hash_key 和 e_hash_key)
5. 同时插入 Internal 和 External 哈希表
6. 创建 `ip_nat_servermap_internal_tuple` 记录原始连接
7. 加入 internal_tuple_list

### 2. 复用映射
1. 同一内部地址:端口的新连接到达
2. 通过 Internal hash 查找已存在的 servermap_node
3. refcnt++
4. 创建新的 internal_tuple 并加入链表
5. inter_tuple_count++

### 3. 查找映射 (入站)
1. 外部流量到达 (e_addr:e_port)
2. 通过 External hash 快速查找 servermap_node
3. 获取对应的内部地址:端口进行反向NAT

### 4. 释放映射
1. 原始连接销毁时，从 internal_tuple_list 删除对应的 tuple
2. refcnt--
3. 当 refcnt==0 且所有连接老化后，设置延迟释放定时器
4. 定时器到期后，从两个哈希表中移除节点并释放内存

## 参考示例图片
本图形化设计参考了以下Linux内核连接跟踪结构的展示风格:
- ip_conntrack 结构图
- ip_conntrack_hash 哈希表图

## 相关代码文件
- 原始数据结构定义: (用户提供的C结构体代码)
- DOT源文件位置: f:\##cfmy-2025\graf\cc-code\
