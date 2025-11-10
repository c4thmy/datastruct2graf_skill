# Data Structure to Graphviz Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude-Code-blue.svg)](https://claude.ai/code)

一个专业的 Claude Code 技能包，用于创建标准化的 Graphviz DOT 数据结构可视化图表。

## 项目简介

本项目将完整的数据结构可视化规范文档打包成 Claude Code 可用的 Skill，让复杂的数据结构可视化变得简单、标准、高效。

### 核心特性

- 🎨 **标准化设计** - 统一的 HTML Table 节点格式和语义化颜色方案
- 🚀 **自动激活** - 自然语言即可触发，无需记忆复杂语法
- 📚 **丰富示例** - 9 种常见数据结构完整模板
- ⚡ **高性能** - 渐进式加载，优化 token 消耗（节省 67%-99%）
- 📖 **文档完备** - 从安装到使用的全流程指导

### 适用场景

- Linux 内核数据结构可视化
- 网络协议栈设计图表
- 哈希表、链表等复杂结构
- NAT、连接跟踪等系统组件
- 技术文档配图
- 代码 Review 辅助

## 项目结构

```
datastruct2graf_skill/
├── cc-code/                                    # 代码文件目录
│   └── graphviz-visualizer/                   # Claude Code Skill 包
│       ├── SKILL.md                           # 核心技能定义
│       ├── README.md                          # Skill 使用说明
│       ├── INSTALLATION.md                    # 安装指南
│       ├── VERIFICATION.md                    # 验证清单
│       ├── references/                        # 参考文档
│       │   ├── style_guide.md                # 完整样式规范
│       │   ├── color_scheme.md               # 颜色方案详解
│       │   └── examples.md                   # 9 种数据结构示例
│       └── assets/                            # 资源文件
│           └── templates/                     # DOT 模板
│               ├── basic_template.dot
│               ├── hash_table_template.dot
│               └── dual_hash_template.dot
├── cc-doc/                                     # 项目文档目录
│   ├── graphviz_data_structure_style_guide.md # 原始完整规范
│   ├── graphviz_skill_packaging_guide.md      # 打包说明文档
│   └── graphviz_skill_qa_record.md            # 完整问答记录
└── README.md                                   # 本文件
```

## 快速开始

### 安装 Skill

#### 方法 1: 手动安装（推荐）

**Windows (PowerShell)**:
```powershell
# 克隆仓库
git clone https://github.com/c4thmy/datastruct2graf_skill.git
cd datastruct2graf_skill

# 复制 Skill 到 Claude Code
Copy-Item -Recurse -Path "cc-code\graphviz-visualizer" `
    -Destination "$env:USERPROFILE\.claude\skills\"
```

**macOS/Linux**:
```bash
# 克隆仓库
git clone https://github.com/c4thmy/datastruct2graf_skill.git
cd datastruct2graf_skill

# 复制 Skill 到 Claude Code
cp -r cc-code/graphviz-visualizer ~/.claude/skills/
```

#### 方法 2: 直接下载

1. 下载本仓库的 ZIP 文件
2. 解压后复制 `cc-code/graphviz-visualizer` 目录到 `~/.claude/skills/`
3. 重启 Claude Code

### 验证安装

重启 Claude Code 后，测试：

```
"请帮我可视化一个哈希表，包含 4 个桶和链表解决冲突"
```

如果生成了标准格式的 DOT 代码，说明安装成功！

## 使用示例

### 示例 1: 简单链表

**输入**:
```
请帮我可视化一个简单的链表，包含 3 个节点
```

**输出**: 标准格式的 DOT 代码，包含：
- lightyellow 链表头
- palegreen 节点
- 绿色 next 连接

### 示例 2: 哈希表

**输入**:
```
创建一个哈希表的 Graphviz 图表，包含水平的桶数组和链表节点
```

**输出**:
- 水平桶数组（lightblue）
- 哈希连接（蓝色粗线）
- 链表节点（palegreen）

### 示例 3: 从结构体生成

**输入**:
```
请为这个结构体创建可视化：
struct hash_node {
    char *key;
    int value;
    struct hash_node *next;
};
```

**输出**: 自动识别字段并生成对应的可视化图表

## 标准颜色方案

| 节点类型 | 颜色 | 适用场景 |
|---------|------|---------|
| 全局指针/入口 | `lightyellow` | 哈希表头、全局变量 |
| Internal/源侧 | `lightblue` | 内部地址、源地址相关 |
| External/目标侧 | `lightcoral` | 外部地址、目标地址相关 |
| 主数据节点 | `palegreen` | 核心数据节点 |
| 辅助结构 | `wheat` | Tuple、Timer 等 |
| 说明/元数据 | `lightgray` | 结构说明、示例 |

## 文档

### 核心文档
- **[SKILL.md](cc-code/graphviz-visualizer/SKILL.md)** - Skill 核心定义和工作流程
- **[README.md](cc-code/graphviz-visualizer/README.md)** - Skill 详细使用说明
- **[INSTALLATION.md](cc-code/graphviz-visualizer/INSTALLATION.md)** - 完整安装指南
- **[VERIFICATION.md](cc-code/graphviz-visualizer/VERIFICATION.md)** - 测试验证清单

### 参考文档
- **[样式指南](cc-code/graphviz-visualizer/references/style_guide.md)** - 完整的 450+ 行技术规范
- **[颜色方案](cc-code/graphviz-visualizer/references/color_scheme.md)** - 颜色使用详解和决策流程
- **[示例库](cc-code/graphviz-visualizer/references/examples.md)** - 9 种常见数据结构完整代码

### 项目文档
- **[打包说明](cc-doc/graphviz_skill_packaging_guide.md)** - Skill 开发过程和设计理念
- **[问答记录](cc-doc/graphviz_skill_qa_record.md)** - 15,000 字完整开发文档
- **[原始规范](cc-doc/graphviz_data_structure_style_guide.md)** - 基础技术规范

## 渲染图表

生成 DOT 代码后，使用 Graphviz 渲染：

```bash
# 生成 SVG（推荐，可缩放）
dot -Tsvg diagram.dot -o diagram.svg

# 生成 PNG
dot -Tpng diagram.dot -o diagram.png

# 生成 PDF
dot -Tpdf diagram.dot -o diagram.pdf
```

### 在线工具

如果没有安装 Graphviz，可使用在线工具：
- [Graphviz Online](https://dreampuf.github.io/GraphvizOnline/)
- [WebGraphviz](http://www.webgraphviz.com/)
- [Edotor](https://edotor.net/)

## 技术特点

### 1. 渐进式加载架构

```
启动时 → 仅加载元数据 (~100 tokens, 节省 99%)
  ↓
激活时 → 加载核心 SKILL.md (~4,000 tokens, 节省 67%)
  ↓
需要时 → 读取详细参考 (~3,000-8,000 tokens/文件)
```

### 2. HTML Table 节点格式

使用 HTML `<TABLE>` 标签而非 record shape：
- 完全控制布局
- 字段垂直排列
- 支持颜色和端口
- 更好的可读性

### 3. 语义化颜色系统

颜色不仅好看，更传递语义信息：
- `lightblue` → Internal（内部/源侧）
- `lightcoral` → External（外部/目标侧）
- 跨图表一致性

### 4. 自动化工作流程

5 步标准流程确保输出质量：
1. 理解需求
2. 规划结构
3. 生成代码
4. 组织格式
5. 输出验证

## 性能指标

| 指标 | 数值 |
|------|------|
| Token 节省（启动） | 99% |
| Token 节省（常规） | 67% |
| 响应时间 | 5-10 秒 |
| 支持数据结构 | 9+ 种 |
| 文档总大小 | ~100 KB |
| 核心文件大小 | 8 KB |

## 贡献

欢迎贡献！你可以：

1. **提交问题** - 报告 bug 或建议新功能
2. **添加示例** - 贡献新的数据结构模板
3. **改进文档** - 修正错误或补充说明
4. **优化代码** - 提高性能或可读性

### 开发指南

如果你想添加新的数据结构示例：

1. Fork 本仓库
2. 在 `cc-code/graphviz-visualizer/references/examples.md` 添加示例
3. 如需要，在 `assets/templates/` 添加模板文件
4. 更新 `README.md`
5. 提交 Pull Request

## 常见问题

### Q: Skill 没有被激活？
**A**: 确保：
- 已复制到 `~/.claude/skills/graphviz-visualizer/`
- `SKILL.md` 文件存在且格式正确
- 已重启 Claude Code

### Q: 生成的代码无法渲染？
**A**:
- 检查 Graphviz 是否安装：`dot -V`
- 使用在线工具测试代码
- 检查 DOT 语法错误

### Q: 如何自定义颜色方案？
**A**: 编辑 `references/color_scheme.md` 和 `SKILL.md` 中的颜色定义

### Q: 可以添加自己的模板吗？
**A**: 可以！在 `assets/templates/` 目录添加新的 `.dot` 文件

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 致谢

- 基于 [Skill_Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) 项目规范
- 使用 [Graphviz](https://graphviz.org/) 可视化引擎
- 为 [Claude Code](https://claude.ai/code) 开发

## 作者

**c4thmy**

- GitHub: [@c4thmy](https://github.com/c4thmy)
- 仓库: [datastruct2graf_skill](https://github.com/c4thmy/datastruct2graf_skill)

## 版本历史

### v1.0.0 (2025-11-10)
- ✨ 初始发布
- ✅ 完整的 Skill 包
- ✅ 9 种数据结构示例
- ✅ 3 个可复用模板
- ✅ 完整文档体系

---

**开始使用**: 立即安装这个 Skill，让 Claude Code 为你创建专业的数据结构可视化图表！

```
"请帮我可视化一个双向链表"
```
