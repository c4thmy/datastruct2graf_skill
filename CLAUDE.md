# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

此项目是一个专业的 Claude Code Skill 包，用于创建标准化的 Graphviz DOT 数据结构可视化图表。

## 项目结构

```
graf/
├── cc-code/                        # 代码文件目录
│   └── graphviz-visualizer/        # Claude Code Skill 包
│       ├── SKILL.md                # 核心技能定义（~8KB，自动加载）
│       ├── README.md               # 完整使用说明
│       ├── INSTALLATION.md         # 安装指南
│       ├── references/             # 参考文档（按需加载）
│       │   ├── style_guide.md     # 完整样式规范（450+ 行）
│       │   ├── color_scheme.md    # 颜色方案详解
│       │   └── examples.md        # 9 种数据结构示例
│       └── assets/templates/       # 可复用 DOT 模板
├── cc-doc/                         # 项目文档目录
│   ├── graphviz_skill_packaging_guide.md    # Skill 打包说明
│   ├── graphviz_skill_qa_record.md          # 完整问答记录（15,000 字）
│   ├── graphviz_data_structure_style_guide.md # 原始规范
│   └── github_release_record.md             # 发布记录
└── dot/                            # 原始 DOT 文件示例
```

## 开发原则

### 文件组织规范
- **cc-code/**: 存放所有可执行代码和 Skill 包，此目录下的内容是项目核心产出
- **cc-doc/**: 存放开发过程文档、技术规范、问答记录等
- **dot/**: 存放原始的 .dot 示例文件，用于测试和参考

### Skill 设计理念
本项目采用**渐进式加载架构**：
- 启动时仅加载 SKILL.md 元数据（~100 tokens，节省 99%）
- 激活时加载核心 SKILL.md 内容（~4,000 tokens，节省 67%）
- 需要时才读取详细参考文档（~3,000-8,000 tokens/文件）

### 核心技术规范

#### HTML Table 节点格式（强制使用）
所有 Graphviz 节点必须使用 HTML `<TABLE>` 标签而非 record shape：
```dot
node_name [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0" BGCOLOR="color">
        <TR><TD PORT="top"><B>结构体名称</B></TD></TR>
        <TR><TD>字段1</TD></TR>
        <TR><TD PORT="field2">字段2</TD></TR>
    </TABLE>
>, shape=plaintext];
```

#### 标准颜色映射（必须遵守）
| 节点类型 | 颜色 | 适用场景 |
|---------|------|---------|
| 全局指针/根节点 | `lightyellow` | 哈希表头、全局变量 |
| Internal/源侧 | `lightblue` | 内部地址、源地址相关 |
| External/目标侧 | `lightcoral` | 外部地址、目标地址相关 |
| 主数据节点 | `palegreen` | 核心数据节点 |
| 辅助结构 | `wheat` | Tuple、Timer 等 |
| 说明/元数据 | `lightgray` | 结构说明框 |

## 常用命令

### 渲染 Graphviz 图表
```bash
# 生成 SVG（推荐，可缩放）
dot -Tsvg input.dot -o output.svg

# 生成 PNG
dot -Tpng input.dot -o output.png

# 生成 PDF
dot -Tpdf input.dot -o output.pdf

# 批量渲染 dot 目录下的所有文件
for file in dot/*.dot; do dot -Tsvg "$file" -o "${file%.dot}.svg"; done
```

### Skill 安装命令

**Windows (PowerShell):**
```powershell
# 复制 Skill 到 Claude Code
Copy-Item -Recurse -Path "cc-code\graphviz-visualizer" `
    -Destination "$env:USERPROFILE\.claude\skills\"
```

**macOS/Linux:**
```bash
# 复制 Skill 到 Claude Code
cp -r cc-code/graphviz-visualizer ~/.claude/skills/
```

### Skill 验证测试
安装后，在 Claude Code 中测试：
```
"请帮我可视化一个哈希表，包含 4 个桶和链表解决冲突"
```

## 架构设计

### Skill 触发机制
SKILL.md 的 frontmatter 定义了自动激活条件：
- 关键词触发："Graphviz"、"DOT"、"data structure visualization"、"可视化"
- 场景触发：内核数据结构、网络协议栈、哈希表、链表等
- 任务触发：将结构体定义转换为可视化图表

### 5 步标准工作流程
1. **理解需求** - 分析结构体定义、数据关系描述
2. **规划结构** - 确定节点类型、层次、颜色方案
3. **生成代码** - 严格遵循 style_guide.md 规范
4. **代码组织** - 分组注释，清晰结构
5. **输出验证** - 确保可执行性和格式规范

### 模板系统
`assets/templates/` 目录提供 3 种基础模板：
- **basic_template.dot** - 通用数据结构模板（6 种节点类型示例）
- **hash_table_template.dot** - 哈希表专用模板（水平桶数组）
- **dual_hash_template.dot** - 双哈希表模板（复杂映射关系）

## 质量标准

### DOT 代码检查清单
生成的代码必须满足：
- ✅ 所有节点标题使用 `<B>` 加粗
- ✅ 关键字段添加了 PORT 属性（用于连接）
- ✅ 颜色选择符合语义映射
- ✅ 连接线有清晰标签
- ✅ 使用 `rankdir=LR` 水平布局（默认）
- ✅ 字体设置为 `Arial, 9pt`
- ✅ 代码有合理的注释分组

### 连接线规范
根据关系类型使用不同样式：
```dot
// 指针指向
node1 -> node2 [label="指向"];

// 哈希链接（蓝色粗线）
hash:bucket -> node:top [color=blue, penwidth=2, label="hash链"];

// 链表连接（绿色）
node1:next -> node2:top [color=green, label="next"];

// 包含关系（虚线）
parent -> child [style=dashed, label="包含"];
```

## 文档维护

### 文档层次关系
- **SKILL.md** - 快速参考，包含核心工作流程（用户最常用）
- **style_guide.md** - 完整技术规范，450+ 行（详细参考）
- **examples.md** - 9 种常见数据结构完整示例代码（学习和复制）
- **color_scheme.md** - 颜色使用决策树（设计选择依据）

### 更新 Skill 时注意
1. **SKILL.md** 修改后需重启 Claude Code 生效
2. **references/** 目录文件可热更新（按需加载）
3. 保持 README.md 与 SKILL.md 内容同步
4. 添加新示例时同时更新 examples.md 和 README.md

## 在线工具

如果没有安装 Graphviz，使用在线渲染工具：
- [Graphviz Online](https://dreampuf.github.io/GraphvizOnline/)
- [WebGraphviz](http://www.webgraphviz.com/)
- [Edotor](https://edotor.net/)

## 参考项目

本 Skill 基于以下标准开发：
- [Skill_Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) - Claude Code Skill 开发规范
- [Graphviz](https://graphviz.org/) - 图形可视化引擎
