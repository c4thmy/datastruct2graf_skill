# Graphviz 数据结构可视化文件索引

## 生成日期
2025-11-10

## 文件清单

### 一、NAT Servermap 数据结构图形文件

#### 1. 完整结构图

##### v1 版本 (record shape)
- **文件**: [ccc-nat_servermap_structure.dot](../cc-code/ccc-nat_servermap_structure.dot)
- **风格**: 使用 record/Mrecord shape
- **布局**: 从左到右 (LR)
- **状态**: ⚠️ 已弃用 (字段无法真正垂直排列)

##### v2 版本 (HTML table) ⭐ 推荐
- **文件**: [ccc-nat_servermap_structure_v2.dot](../cc-code/ccc-nat_servermap_structure_v2.dot)
- **风格**: 使用 HTML `<TABLE>` 标签
- **布局**: 从左到右 (LR)
- **特点**:
  - 字段完全垂直排列
  - 支持精确端口连接
  - 符合统一风格规范
- **状态**: ✅ 当前版本，推荐使用

#### 2. 哈希链式结构图

##### v1 版本 (record shape)
- **文件**: [ccc-nat_servermap_hash_chain.dot](../cc-code/ccc-nat_servermap_hash_chain.dot)
- **风格**: 混合 record 和 Mrecord shape
- **布局**: 从左到右 (LR)
- **状态**: ⚠️ 已弃用

##### v2 版本 (HTML table) ⭐ 推荐
- **文件**: [ccc-nat_servermap_hash_chain_v2.dot](../cc-code/ccc-nat_servermap_hash_chain_v2.dot)
- **风格**: 完全使用 HTML `<TABLE>` 标签
- **布局**: 从左到右 (LR)
- **特点**:
  - 展示双哈希表索引机制
  - 哈希桶数组水平显示
  - 节点字段垂直排列
- **状态**: ✅ 当前版本，推荐使用

---

### 二、文档文件

#### 1. 通用风格规范 ⭐ 重要
- **文件**: [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)
- **内容**:
  - 设计原则和统一标准
  - HTML Table 语法规范
  - 标准颜色方案
  - 连接线规范
  - 特殊结构处理方法
  - 完整示例模板
  - 最佳实践和常见问题
- **用途**: **所有数据结构可视化的统一参考文档**

#### 2. v2 版本说明
- **文件**: [nat_servermap_structure_v2_notes.md](../cc-doc/nat_servermap_structure_v2_notes.md)
- **内容**:
  - v1 vs v2 版本对比
  - v2 版本特点详解
  - 使用方法和生成命令

#### 3. 原始说明文档
- **文件**: [nat_servermap_structure_visualization.md](../cc-doc/nat_servermap_structure_visualization.md)
- **内容**:
  - 原始版本的说明
  - 数据结构关键点
  - 工作流程说明

---

## 使用指南

### 快速开始

#### 1. 生成图片
```bash
# 使用 v2 版本生成 PNG
dot -Tpng cc-code/ccc-nat_servermap_structure_v2.dot -o nat_structure.png
dot -Tpng cc-code/ccc-nat_servermap_hash_chain_v2.dot -o nat_hash.png

# 生成 SVG (推荐，可缩放)
dot -Tsvg cc-code/ccc-nat_servermap_structure_v2.dot -o nat_structure.svg
dot -Tsvg cc-code/ccc-nat_servermap_hash_chain_v2.dot -o nat_hash.svg
```

#### 2. 在线预览
访问以下任一网站，复制 DOT 文件内容即可预览:
- https://dreampuf.github.io/GraphvizOnline/
- http://www.webgraphviz.com/
- https://edotor.net/

#### 3. 创建新的数据结构图
1. 参考 [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md)
2. 使用其中的模板代码
3. 遵循统一的颜色和风格规范
4. 保存为 `.dot` 文件

---

## 风格规范摘要

### 核心原则
✅ **使用 HTML `<TABLE>` 标签**，不使用 record shape
✅ **布局方向**: `rankdir=LR` (从左到右)
✅ **字段排列**: 每个字段独立一行 (`<TR><TD>`)
✅ **统一颜色**: 按功能使用标准颜色方案

### 标准颜色

| 用途 | 颜色 |
|------|------|
| 全局指针 | `lightyellow` |
| Internal/源侧 | `lightblue` |
| External/目标侧 | `lightcoral` |
| 主节点 | `palegreen` |
| 辅助结构 | `wheat` |
| 说明 | `lightgray` |

### 节点模板

```dot
node_name [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="palegreen">
        <TR><TD PORT="top"><B>结构体名</B></TD></TR>
        <TR><TD PORT="field1">字段1</TD></TR>
        <TR><TD>字段2</TD></TR>
        <TR><TD>字段3</TD></TR>
    </TABLE>
>, shape=plaintext];
```

### 连接线规范

```dot
node1:port1 -> node2:port2 [label="说明", color=blue, penwidth=2];
```

---

## 项目结构

```
graf/
├── cc-code/                    # DOT 源文件目录
│   ├── ccc-nat_servermap_structure.dot         [已弃用]
│   ├── ccc-nat_servermap_structure_v2.dot      [✅ 使用]
│   ├── ccc-nat_servermap_hash_chain.dot        [已弃用]
│   └── ccc-nat_servermap_hash_chain_v2.dot     [✅ 使用]
│
├── cc-doc/                     # 文档目录
│   ├── graphviz_data_structure_style_guide.md  [⭐ 通用规范]
│   ├── nat_servermap_structure_v2_notes.md
│   ├── nat_servermap_structure_visualization.md
│   └── index.md                                [本文件]
│
└── dot/                        # 原始参考文件
    └── snat3.dot
```

---

## 版本对比

| 特性 | v1 (record) | v2 (HTML table) |
|------|-------------|-----------------|
| 字段排列 | ❌ 水平 | ✅ 垂直 |
| 端口定义 | ✅ 支持 | ✅ 完全支持 |
| 颜色控制 | ⚠️ 有限 | ✅ 完全控制 |
| 对齐方式 | ❌ 受限 | ✅ 灵活 |
| 复杂布局 | ❌ 困难 | ✅ 容易 |
| **推荐度** | ❌ | ✅✅✅ |

---

## 适用场景

### NAT Servermap 数据结构
- ✅ 哈希表结构
- ✅ 双向索引机制
- ✅ 链表关系
- ✅ 复用映射关系

### 通用数据结构 (使用规范文档)
- ✅ 内核链表
- ✅ 红黑树
- ✅ 哈希表
- ✅ 网络协议栈
- ✅ 连接跟踪表
- ✅ 任何复杂 C 数据结构

---

## 工具安装

### Windows
```powershell
# 使用 Chocolatey
choco install graphviz

# 或下载安装包
# https://graphviz.org/download/
```

### Linux
```bash
# Ubuntu/Debian
sudo apt install graphviz

# CentOS/RHEL
sudo yum install graphviz
```

### macOS
```bash
brew install graphviz
```

---

## 贡献指南

### 添加新的数据结构图形
1. 遵循 [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md) 规范
2. 文件命名: `ccc-<结构名>_<类型>.dot`
3. 保存到 `cc-code/` 目录
4. 更新本索引文件

### 改进建议
如发现规范文档不完善或有更好的实践方法，请：
1. 更新规范文档
2. 更新现有 DOT 文件以保持一致
3. 记录版本变更

---

## 参考资源

### 官方文档
- Graphviz 官网: https://graphviz.org/
- DOT 语言参考: https://graphviz.org/doc/info/lang.html
- 节点形状: https://graphviz.org/doc/info/shapes.html
- HTML-like 标签: https://graphviz.org/doc/info/shapes.html#html

### 在线工具
- GraphvizOnline: https://dreampuf.github.io/GraphvizOnline/
- WebGraphviz: http://www.webgraphviz.com/
- Edotor: https://edotor.net/

---

## 常见问题

### Q: 为什么要统一使用 HTML table?
**A**: HTML table 提供完全的垂直布局控制，符合表格化展示需求，且支持精确的端口定义和颜色控制。

### Q: 如何选择颜色?
**A**: 参考 [graphviz_data_structure_style_guide.md](../cc-doc/graphviz_data_structure_style_guide.md) 的标准颜色方案，保持一致性。

### Q: 生成的图片太大或太小?
**A**: 调整 `fontsize` 参数，或使用 `-Gdpi=150` 参数控制 DPI:
```bash
dot -Tpng -Gdpi=150 input.dot -o output.png
```

### Q: 如何在 VSCode 中预览?
**A**: 安装 "Graphviz Preview" 扩展，打开 `.dot` 文件后按 `Ctrl+Shift+P` 输入 "Graphviz: Preview"。

---

**文档维护**: 本索引文件应随着新图形和文档的添加而更新
**最后更新**: 2025-11-10
