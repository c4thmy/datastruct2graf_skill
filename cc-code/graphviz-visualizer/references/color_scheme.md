# Graphviz 数据结构可视化 - 标准颜色方案

## 颜色映射表

| 用途 | 颜色名 | 十六进制 | RGB | 适用场景 |
|------|--------|---------|-----|---------|
| **全局指针/入口** | `lightyellow` | #FFFFE0 | (255,255,224) | 全局变量、哈希表头指针、根节点 |
| **Internal/源侧** | `lightblue` | #ADD8E6 | (173,216,230) | 内部地址、源地址、内网相关 |
| **External/目标侧** | `lightcoral` | #F08080 | (240,128,128) | 外部地址、目标地址、外网相关 |
| **主节点/实体** | `palegreen` | #98FB98 | (152,251,152) | 核心数据节点、主要结构体 |
| **辅助结构** | `wheat` | #F5DEB3 | (245,222,179) | Tuple、Timer、辅助数据 |
| **说明/元数据** | `lightgray` | #D3D3D3 | (211,211,211) | 结构说明、元信息、注释框 |

## 颜色使用原则

### 1. 语义一致性
同类型的结构体在不同图表中应使用相同颜色：
```dot
// NAT 系统示例
internal_node [BGCOLOR="lightblue"]    // 所有内部地址节点
external_node [BGCOLOR="lightcoral"]   // 所有外部地址节点
mapping_node [BGCOLOR="palegreen"]     // 映射记录节点
```

### 2. 对比性
相关但不同的结构使用对比色：
- Internal vs External: `lightblue` vs `lightcoral`
- 主结构 vs 辅助结构: `palegreen` vs `wheat`
- 数据 vs 说明: 彩色 vs `lightgray`

### 3. 层次性
```
lightyellow (入口层)
    ↓
lightblue/lightcoral (分类层)
    ↓
palegreen (数据层)
    ↓
wheat (辅助层)
```

### 4. 可读性原则
- ✅ 所有颜色都是浅色背景
- ✅ 确保黑色文字清晰可读
- ❌ 避免使用深色背景（如 darkblue, darkgreen）
- ❌ 避免使用高饱和度颜色（如 red, yellow）

## 实际应用示例

### 哈希表系统
```dot
digraph HashTable {
    // 入口节点 - 浅黄色
    hash_head [BGCOLOR="lightyellow"];

    // 桶数组 - 浅蓝色（内部结构）
    buckets [BGCOLOR="lightblue"];

    // 数据节点 - 浅绿色
    node1 [BGCOLOR="palegreen"];
    node2 [BGCOLOR="palegreen"];

    // 辅助信息 - 小麦色
    timer [BGCOLOR="wheat"];
}
```

### 网络协议栈
```dot
digraph NetworkStack {
    // 应用层接口 - 浅黄色
    app_interface [BGCOLOR="lightyellow"];

    // 发送路径 - 浅蓝色
    tx_buffer [BGCOLOR="lightblue"];

    // 接收路径 - 浅珊瑚色
    rx_buffer [BGCOLOR="lightcoral"];

    // 数据包 - 浅绿色
    packet [BGCOLOR="palegreen"];

    // 统计信息 - 浅灰色
    stats [BGCOLOR="lightgray"];
}
```

### 双向链表
```dot
digraph LinkedList {
    // 链表头 - 浅黄色
    list_head [BGCOLOR="lightyellow"];

    // 节点 - 浅绿色
    node1 [BGCOLOR="palegreen"];
    node2 [BGCOLOR="palegreen"];
    node3 [BGCOLOR="palegreen"];

    // 占位符 - 浅灰色
    more [label="......", BGCOLOR="lightgray"];
}
```

## 连接线颜色

| 连接类型 | 颜色 | 线宽 | 样式 | 说明 |
|---------|------|------|------|------|
| 默认指针 | `black` | 1 | solid | 普通指针关系 |
| Internal 哈希链 | `blue` | 2 | solid | 内部地址哈希 |
| External 哈希链 | `red` | 2 | solid | 外部地址哈希 |
| 链表 next | `green` | 1-2 | solid | 前向指针 |
| 链表 prev | `red` | 1 | solid | 后向指针 |
| 包含关系 | `black` | 1 | dashed | 逻辑包含 |
| 说明引用 | `gray` | 1 | dotted | 注释说明 |

## 颜色组合建议

### 推荐组合
✅ `lightblue` + `lightcoral` - 双向/对偶结构（如 NAT）
✅ `lightyellow` + `palegreen` - 入口与数据
✅ `palegreen` + `wheat` - 主数据与辅助
✅ `lightgray` - 说明框（通用）

### 避免组合
❌ `lightblue` + `palegreen` - 缺乏对比
❌ `lightcoral` + `wheat` - 颜色太接近
❌ 超过 4 种主要颜色 - 过于复杂

## 颜色决策流程图

```
开始
  ↓
是全局入口/根节点？ → 是 → lightyellow
  ↓ 否
有明确方向性？
  ↓ 是
  源侧/内部/发送方？ → 是 → lightblue
  目标侧/外部/接收方？ → 是 → lightcoral
  ↓ 否
是主要数据结构？ → 是 → palegreen
  ↓ 否
是辅助结构？ → 是 → wheat
  ↓ 否
是说明/元数据？ → 是 → lightgray
  ↓ 否
默认使用 → palegreen
```

## 检查清单

在生成图表前确认：
- [ ] 每种颜色的使用是否有明确语义
- [ ] 同类节点是否使用相同颜色
- [ ] 颜色对比是否足够清晰
- [ ] 连接线颜色是否与节点颜色协调
- [ ] 图表中主要颜色是否不超过 4-5 种
- [ ] 文字在所有背景色上是否清晰可读

## 扩展颜色（可选）

如需更多颜色，按此顺序添加：
1. `lightcyan` (#E0FFFF) - 次要内部结构
2. `lightsalmon` (#FFA07A) - 次要外部结构
3. `lightsteelblue` (#B0C4DE) - 缓存/临时结构
4. `lemonchiffon` (#FFFACD) - 配置/策略

**注意**: 避免无限扩展颜色方案，保持简洁性。
