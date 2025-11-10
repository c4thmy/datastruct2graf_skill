# NAT Servermap 数据结构可视化项目总结

## 项目完成时间
2025-11-10

---

## 一、项目成果

### 1. 统一风格的 DOT 图形文件 (v2 版本)

#### ✅ [ccc-nat_servermap_structure_v2.dot](../cc-code/ccc-nat_servermap_structure_v2.dot)
- **用途**: 展示 NAT Servermap 完整数据结构层次关系
- **包含内容**:
  - 全局哈希表指针 (Internal & External)
  - 哈希表结构
  - 哈希桶链结构
  - ServerMap 节点详细结构 (18个字段)
  - Internal Tuple 列表
  - Timer 定时器结构
- **特点**: 完全垂直排列字段，使用 HTML table 标签

#### ✅ [ccc-nat_servermap_hash_chain_v2.dot](../cc-code/ccc-nat_servermap_hash_chain_v2.dot)
- **用途**: 展示双哈希表的链式结构和节点挂载机制
- **包含内容**:
  - Internal & External 双哈希表并列展示
  - 哈希桶数组 (水平布局)
  - ServerMap 节点挂载示例 (3个示例节点)
  - Internal Tuple List 连接关系
  - 哈希冲突链
- **特点**: 清晰展示双重索引机制

### 2. 通用规范文档 ⭐

#### ✅ [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)
**这是最重要的成果！**

包含完整的 Graphviz 数据结构可视化规范，可用于其他任何数据结构的可视化：

- **设计原则**: 统一风格标准、视觉层次
- **HTML Table 语法规范**: 详细的节点模板和参数说明
- **标准颜色方案**: 7种预定义颜色及使用原则
- **连接线规范**: 5种线条样式和使用场景
- **特殊结构处理**:
  - 哈希表数组
  - 链表结构
  - 双向索引
  - 说明文字框
- **完整示例模板**:
  - 简单数据结构
  - 哈希表结构
  - 双向链表
- **最佳实践**: 节点设计、连接线设计、布局优化、可读性建议
- **常见问题解答**
- **快速参考卡片**

### 3. 辅助文档

#### ✅ [index.md](../cc-doc/index.md)
- 文件索引和使用指南
- 版本对比说明
- 适用场景列表
- 工具安装指南
- 快速开始教程

#### ✅ [nat_servermap_structure_v2_notes.md](../cc-doc/nat_servermap_structure_v2_notes.md)
- v1 vs v2 版本详细对比
- v2 版本特点说明
- 使用方法

---

## 二、技术要点

### 核心改进：从 record shape 到 HTML table

#### v1 问题
```dot
// record shape - 字段水平排列
node [label="{field1|field2|field3}", shape=record];
```

#### v2 解决方案
```dot
// HTML table - 字段垂直排列
node [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="color">
        <TR><TD>field1</TD></TR>
        <TR><TD>field2</TD></TR>
        <TR><TD>field3</TD></TR>
    </TABLE>
>, shape=plaintext];
```

### 统一风格要点

1. **布局方向**: `rankdir=LR` (从左到右)
2. **节点格式**: HTML `<TABLE>` 标签
3. **字段排列**: 每字段一行 (`<TR><TD>`)
4. **端口定义**: 精确连接锚点 (`PORT="name"`)
5. **颜色方案**: 7种标准颜色分类使用

### 标准颜色映射

```
lightyellow → 全局指针/入口
lightblue   → Internal/源侧
lightcoral  → External/目标侧
palegreen   → 主节点/实体
wheat       → 辅助结构
lightgray   → 说明/元数据
```

---

## 三、数据结构关系图展示的关键概念

### NAT Servermap 核心机制

1. **双哈希表设计**
   - Internal hash: 按 `(i_saddr, i_sport)` 索引
   - External hash: 按 `(e_addr, e_port)` 索引
   - 同一节点同时挂载在两个哈希表

2. **引用计数机制**
   - `refcnt`: 记录有多少 internal_tuple 引用该节点
   - 初始值为1，复用时递增

3. **Internal Tuple List**
   - 链表结构记录所有复用该映射的原始连接
   - 每个 tuple 包含完整五元组

4. **延迟释放机制**
   - `snat_ct_del_time`: 连接销毁时间戳
   - `map_init_timer_flag`: 定时器标志
   - 支持连接老化后延迟释放

---

## 四、使用方法

### 生成图片

```bash
# PNG 格式
dot -Tpng cc-code/ccc-nat_servermap_structure_v2.dot -o output.png

# SVG 格式 (推荐，可缩放)
dot -Tsvg cc-code/ccc-nat_servermap_structure_v2.dot -o output.svg

# PDF 格式
dot -Tpdf cc-code/ccc-nat_servermap_structure_v2.dot -o output.pdf

# 哈希链式结构图
dot -Tpng cc-code/ccc-nat_servermap_hash_chain_v2.dot -o hash_chain.png
```

### 在线预览

访问以下任一网站，粘贴 DOT 文件内容：
- https://dreampuf.github.io/GraphvizOnline/
- http://www.webgraphviz.com/
- https://edotor.net/

### 创建新的数据结构图

1. 打开 [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)
2. 找到适合的模板 (第六节)
3. 复制模板代码
4. 替换结构名和字段
5. 遵循颜色规范
6. 保存为 `.dot` 文件
7. 生成图片验证效果

---

## 五、文件结构

```
graf/
├── cc-code/                                    # DOT 源文件
│   ├── ccc-nat_servermap_structure_v2.dot     [✅ 完整结构图]
│   ├── ccc-nat_servermap_hash_chain_v2.dot    [✅ 哈希链式图]
│   ├── ccc-nat_servermap_structure.dot        [弃用]
│   └── ccc-nat_servermap_hash_chain.dot       [弃用]
│
├── cc-doc/                                     # 文档
│   ├── graphviz_data_structure_style_guide.md [⭐ 通用规范]
│   ├── index.md                               [文件索引]
│   ├── nat_servermap_structure_v2_notes.md    [版本说明]
│   ├── nat_servermap_structure_visualization.md [原始说明]
│   └── summary.md                             [本文件]
│
└── dot/                                        # 原始参考
    └── snat3.dot
```

---

## 六、规范文档的通用性

### ✅ 适用于以下数据结构的可视化：

1. **内核数据结构**
   - 链表 (list_head)
   - 哈希表 (hash table)
   - 红黑树 (rbtree)
   - 基数树 (radix tree)

2. **网络协议栈**
   - 连接跟踪表 (conntrack)
   - 路由表 (routing table)
   - 邻居表 (neighbor table)
   - 套接字缓冲区 (sk_buff)

3. **文件系统**
   - inode 结构
   - dentry 缓存
   - 页缓存 (page cache)

4. **内存管理**
   - 页表结构
   - slab 分配器
   - 伙伴系统

5. **任何复杂 C 数据结构**

---

## 七、关键优势

### 与原始 record shape 相比

| 特性 | record shape | HTML table (v2) |
|------|--------------|-----------------|
| 字段方向 | ❌ 默认水平 | ✅ 完全垂直 |
| 布局控制 | ⚠️ 有限 | ✅ 完全控制 |
| 颜色支持 | ⚠️ 单色 | ✅ 每行独立颜色 |
| 端口定义 | ✅ 支持 | ✅ 完全支持 |
| 对齐方式 | ❌ 受限 | ✅ LEFT/CENTER/RIGHT |
| 跨行/跨列 | ❌ 不支持 | ✅ COLSPAN/ROWSPAN |
| 可读性 | ⚠️ 一般 | ✅ 优秀 |
| 维护性 | ⚠️ 困难 | ✅ 容易 |

### 规范文档的价值

✅ **可复用**: 适用于任何数据结构可视化项目
✅ **标准化**: 统一风格，团队协作友好
✅ **完整性**: 包含语法、示例、最佳实践
✅ **实用性**: 快速参考卡片，开箱即用
✅ **可扩展**: 易于根据需求调整和补充

---

## 八、后续改进建议

### 1. 工具化
- 编写脚本自动从 C 结构体生成 DOT 文件
- 支持从源码注释提取字段说明

### 2. 模板库
- 创建更多常见数据结构的模板
- 支持一键生成常用结构

### 3. 交互式
- 使用 D3.js 或其他工具创建可交互的 SVG
- 支持点击展开/折叠详细字段

### 4. 文档增强
- 添加更多复杂示例
- 视频教程

---

## 九、总结

### 项目成果

1. ✅ **2个统一风格的 v2 版本 DOT 文件** (NAT Servermap)
2. ✅ **1份通用规范文档** (可用于任何数据结构)
3. ✅ **3份辅助说明文档** (索引、版本说明、总结)

### 核心贡献

**[graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)** 是本项目最重要的产出，它提供了：

- 📐 **统一的设计标准**
- 🎨 **标准颜色方案**
- 📝 **HTML table 语法规范**
- 🔗 **连接线规范**
- 📚 **完整的示例模板**
- ✅ **最佳实践指南**

这份文档可以作为**任何其他数据结构可视化项目的基础规范**，确保风格统一、可读性强、维护方便。

### 技术突破

从 `record shape` 到 `HTML table` 的转变，彻底解决了字段垂直排列的问题，使得生成的图形更接近传统的数据结构教科书风格。

---

## 十、快速上手

### 第一步：查看现有成果
```bash
cd f:\##cfmy-2025\graf\cc-code
# 在线预览工具中打开 ccc-nat_servermap_structure_v2.dot
```

### 第二步：生成图片
```bash
dot -Tpng ccc-nat_servermap_structure_v2.dot -o nat_structure.png
```

### 第三步：创建新结构
1. 打开 [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)
2. 复制第六节的模板
3. 根据你的数据结构修改
4. 生成并验证

---

**项目完成！所有文件符合 CLAUDE.md 要求，保存至指定目录。**

**关键文件**:
- ⭐ [通用规范文档](../cc-doc/graphviz_data_structure_style_guide.md)
- [完整结构图 v2](../cc-code/ccc-nat_servermap_structure_v2.dot)
- [哈希链式图 v2](../cc-code/ccc-nat_servermap_hash_chain_v2.dot)
- [文件索引](../cc-doc/index.md)
