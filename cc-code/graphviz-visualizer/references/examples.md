# Graphviz 数据结构可视化 - 常见示例

本文档提供常见数据结构的标准 Graphviz 实现模板。

---

## 1. 简单链表

```dot
digraph SimpleLinkedList {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 链表头
    head [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD PORT="h"><B>list_head</B></TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 节点
    node1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>list_node</B></TD></TR>
            <TR><TD>data: value1</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    node2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>list_node</B></TD></TR>
            <TR><TD>data: value2</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    node3 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>list_node</B></TD></TR>
            <TR><TD>data: value3</TD></TR>
            <TR><TD PORT="next">next: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    head:h -> node1:top [label="指向"];
    node1:next -> node2:top [label="next", color=green];
    node2:next -> node3:top [label="next", color=green];
}
```

---

## 2. 双向链表

```dot
digraph DoublyLinkedList {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 链表头
    head [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD PORT="h"><B>dlist_head</B></TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 节点
    node1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>dlist_node</B></TD></TR>
            <TR><TD>data: A</TD></TR>
            <TR><TD PORT="prev">prev</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    node2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>dlist_node</B></TD></TR>
            <TR><TD>data: B</TD></TR>
            <TR><TD PORT="prev">prev</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    head:h -> node1:top [label="head"];
    node1:next -> node2:top [label="next", color=green];
    node2:prev -> node1:next [label="prev", color=red, dir=back];
}
```

---

## 3. 哈希表（拉链法）

```dot
digraph HashTable {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 哈希表头
    ht_head [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD><B>hash_table</B></TD></TR>
            <TR><TD>size: 256</TD></TR>
            <TR><TD>buckets[]</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 桶数组
    buckets [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightblue">
            <TR>
                <TD PORT="b0">bucket[0]</TD>
                <TD PORT="b1">bucket[1]</TD>
                <TD PORT="b2">bucket[2]</TD>
                <TD>......</TD>
                <TD PORT="bn">bucket[255]</TD>
            </TR>
        </TABLE>
    >, shape=plaintext];

    // 链表节点
    node1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>hash_node</B></TD></TR>
            <TR><TD>key: "foo"</TD></TR>
            <TR><TD>value: 100</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    node2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>hash_node</B></TD></TR>
            <TR><TD>key: "bar"</TD></TR>
            <TR><TD>value: 200</TD></TR>
            <TR><TD PORT="next">next: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    ht_head -> buckets [label="指向"];
    buckets:b2 -> node1:top [color=blue, penwidth=2];
    node1:next -> node2:top [color=green, label="next"];
}
```

---

## 4. 二叉树

```dot
digraph BinaryTree {
    rankdir=TB;  // 树结构用自上而下
    node [fontname="Arial", fontsize=9];

    // 根节点
    root [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD PORT="top"><B>tree_root</B></TD></TR>
            <TR><TD>value: 10</TD></TR>
            <TR><TD PORT="left">left</TD></TR>
            <TR><TD PORT="right">right</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 左子节点
    left [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>tree_node</B></TD></TR>
            <TR><TD>value: 5</TD></TR>
            <TR><TD PORT="left">left</TD></TR>
            <TR><TD PORT="right">right</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 右子节点
    right [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>tree_node</B></TD></TR>
            <TR><TD>value: 15</TD></TR>
            <TR><TD PORT="left">left: NULL</TD></TR>
            <TR><TD PORT="right">right: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    root:left -> left:top [label="left"];
    root:right -> right:top [label="right"];
}
```

---

## 5. 双哈希表结构（NAT Servermap 风格）

```dot
digraph DualHashTable {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // Internal 哈希表
    inter_hash [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightblue">
            <TR>
                <TD PORT="h0">bucket[0]</TD>
                <TD PORT="h1">bucket[1]</TD>
                <TD>......</TD>
                <TD PORT="hn">bucket[n]</TD>
            </TR>
        </TABLE>
    >, shape=plaintext];

    // External 哈希表
    ext_hash [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightcoral">
            <TR>
                <TD PORT="h0">bucket[0]</TD>
                <TD PORT="h1">bucket[1]</TD>
                <TD>......</TD>
                <TD PORT="hn">bucket[n]</TD>
            </TR>
        </TABLE>
    >, shape=plaintext];

    // Mapping 节点
    mapping [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top" COLSPAN="2"><B>servermap_node</B></TD></TR>
            <TR><TD PORT="inter_hash">inter_hash_node</TD><TD>内部地址索引</TD></TR>
            <TR><TD PORT="ext_hash">ext_hash_node</TD><TD>外部地址索引</TD></TR>
            <TR><TD>internal_ip</TD><TD>192.168.1.100</TD></TR>
            <TR><TD>external_ip</TD><TD>203.0.113.5</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    inter_hash:h1 -> mapping:inter_hash [color=blue, penwidth=2, label="内部hash"];
    ext_hash:h2 -> mapping:ext_hash [color=red, penwidth=2, label="外部hash"];
}
```

---

## 6. 栈结构

```dot
digraph Stack {
    rankdir=TB;
    node [fontname="Arial", fontsize=9];

    // 栈头
    top [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD PORT="t"><B>stack_top</B></TD></TR>
            <TR><TD>size: 3</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 栈元素
    elem1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>stack_elem</B></TD></TR>
            <TR><TD>data: 30</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    elem2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>stack_elem</B></TD></TR>
            <TR><TD>data: 20</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    elem3 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>stack_elem</B></TD></TR>
            <TR><TD>data: 10</TD></TR>
            <TR><TD PORT="next">next: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    top:t -> elem1:top [label="top"];
    elem1:next -> elem2:top [label="next"];
    elem2:next -> elem3:top [label="next"];
}
```

---

## 7. 队列结构

```dot
digraph Queue {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 队列头尾指针
    queue [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD PORT="head"><B>queue_head</B></TD></TR>
            <TR><TD PORT="tail"><B>queue_tail</B></TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 队列元素
    elem1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>queue_elem</B></TD></TR>
            <TR><TD>data: A</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    elem2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>queue_elem</B></TD></TR>
            <TR><TD>data: B</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    elem3 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>queue_elem</B></TD></TR>
            <TR><TD>data: C</TD></TR>
            <TR><TD PORT="next">next: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    queue:head -> elem1:top [label="head"];
    queue:tail -> elem3:top [label="tail"];
    elem1:next -> elem2:top [label="next", color=green];
    elem2:next -> elem3:top [label="next", color=green];
}
```

---

## 8. 图结构（邻接表）

```dot
digraph Graph {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 顶点数组
    vertices [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR>
                <TD PORT="v0">Vertex A</TD>
                <TD PORT="v1">Vertex B</TD>
                <TD PORT="v2">Vertex C</TD>
            </TR>
        </TABLE>
    >, shape=plaintext];

    // A 的邻接表
    edge_a1 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>edge</B></TD></TR>
            <TR><TD>to: B</TD></TR>
            <TR><TD>weight: 5</TD></TR>
            <TR><TD PORT="next">next</TD></TR>
        </TABLE>
    >, shape=plaintext];

    edge_a2 [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>edge</B></TD></TR>
            <TR><TD>to: C</TD></TR>
            <TR><TD>weight: 3</TD></TR>
            <TR><TD PORT="next">next: NULL</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    vertices:v0 -> edge_a1:top [label="edges"];
    edge_a1:next -> edge_a2:top [label="next", color=green];
}
```

---

## 9. 带说明框的复杂结构

```dot
digraph ComplexStructure {
    rankdir=LR;
    node [fontname="Arial", fontsize=9];

    // 主结构
    main [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
            <TR><TD PORT="top"><B>complex_struct</B></TD></TR>
            <TR><TD>field1</TD></TR>
            <TR><TD PORT="ptr">pointer</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 辅助结构
    aux [label=<
        <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="wheat">
            <TR><TD PORT="top"><B>aux_struct</B></TD></TR>
            <TR><TD>data</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 说明框
    note [label=<
        <TABLE BORDER="1" CELLBORDER="0" CELLSPACING="0" BGCOLOR="lightyellow">
            <TR><TD ALIGN="LEFT"><B>关键说明:</B></TD></TR>
            <TR><TD ALIGN="LEFT">• pointer 字段指向辅助结构</TD></TR>
            <TR><TD ALIGN="LEFT">• 辅助结构可被多个主结构共享</TD></TR>
            <TR><TD ALIGN="LEFT">• 使用引用计数管理生命周期</TD></TR>
        </TABLE>
    >, shape=plaintext];

    // 连接
    main:ptr -> aux:top [label="指向"];
    note -> aux [style=dotted, color=gray];
}
```

---

## 使用建议

1. **选择合适模板**: 根据数据结构类型选择最接近的模板
2. **修改字段内容**: 替换示例数据为实际字段
3. **调整颜色**: 根据语义选择合适的背景色
4. **添加 PORT**: 为需要连接的字段添加端口
5. **测试渲染**: 使用 `dot -Tsvg` 测试输出

## 渲染命令

```bash
# 保存代码为 example.dot，然后执行：
dot -Tsvg example.dot -o example.svg
```
