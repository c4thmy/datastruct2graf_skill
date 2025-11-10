# NAT Servermap 数据结构图更新说明

## 版本对比

### v1 版本 (ccc-nat_servermap_structure.dot)
- 使用 record/Mrecord shape
- 字段使用 `|` 分隔符，但默认是水平排列
- 无法实现真正的垂直表格效果

### v2 版本 (ccc-nat_servermap_structure_v2.dot) - 推荐使用
- **使用 HTML-like table 标签**
- 每个字段独立成行，完全垂直排列
- 完美匹配示例图片的表格风格
- 支持端口（PORT）定义，连接箭头更精确

## v2 版本特点

### 1. HTML Table 语法
```dot
node [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="color">
        <TR><TD>字段1</TD></TR>
        <TR><TD>字段2</TD></TR>
        <TR><TD>字段3</TD></TR>
    </TABLE>
>, shape=plaintext];
```

### 2. 垂直布局效果
- 每个 `<TR><TD>` 对应一行
- 所有字段严格从上到下排列
- 类似Excel表格的显示效果

### 3. 端口定义
```dot
<TR><TD PORT="portname">字段内容</TD></TR>
```
可以精确指定箭头连接到哪一行

### 4. 颜色区分
- lightyellow: 全局指针
- lightblue: Internal 哈希相关
- lightcoral: External 哈希相关
- palegreen: ServerMap 节点
- wheat: Tuple 和 Timer

## 生成命令

```bash
# PNG 格式
dot -Tpng ccc-nat_servermap_structure_v2.dot -o nat_servermap_structure_v2.png

# SVG 格式（推荐，可缩放）
dot -Tsvg ccc-nat_servermap_structure_v2.dot -o nat_servermap_structure_v2.svg

# PDF 格式
dot -Tpdf ccc-nat_servermap_structure_v2.dot -o nat_servermap_structure_v2.pdf
```

## 数据结构完整展示

v2 版本完整展示了以下结构的所有字段（垂直排列）：

1. **全局哈希表指针**
   - g_nsm_by_address_hash_table_inter
   - g_nsm_by_address_hash_table_ext

2. **哈希表结构**
   - ip_nat_servermap_hash_table_inter
   - ip_nat_servermap_hash_table_ext

3. **哈希桶链 (每个包含4个字段)**
   - ip_nat_servermap_hash_chain
   - nsm_hlist
   - nsmh_lock
   - count

4. **ServerMap节点 (18个字段垂直排列)**
   - nsm_node_by_ext_address_hash
   - nsm_node_by_internal_address_hash
   - internal_tuple_list
   - timer
   - servermapping_lock
   - snat_ct_del_time
   - e_addr / e_port
   - i_saddr / i_sport
   - refcnt
   - inter_tuple_count
   - i_hash_key / e_hash_key
   - entry_vsys_id / entry_id
   - protonum
   - map_init_timer_flag

5. **Internal Tuple (9个字段垂直排列)**
   - node
   - tuple
   - src (IP + port)
   - dst (IP + port)

6. **Timer结构**
   - mapping_node_timer
   - expires

## 推荐使用 v2 版本

v2 版本使用 HTML table 实现了真正的垂直表格布局，完美匹配您提供的示例图片风格。
