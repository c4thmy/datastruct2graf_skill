# Graphviz Visualizer Skill - 安装指南

## 快速安装（3 步完成）

### 步骤 1: 找到 Claude Code Skills 目录

Claude Code 的 skills 默认位置：

**Windows:**
```powershell
%USERPROFILE%\.claude\skills\
# 通常是: C:\Users\你的用户名\.claude\skills\
```

**macOS/Linux:**
```bash
~/.claude/skills/
# 完整路径: /Users/你的用户名/.claude/skills/ (macOS)
#          /home/你的用户名/.claude/skills/ (Linux)
```

如果目录不存在，手动创建：
```bash
# Windows (PowerShell)
mkdir $env:USERPROFILE\.claude\skills

# macOS/Linux
mkdir -p ~/.claude/skills
```

### 步骤 2: 复制 Skill 目录

将整个 `graphviz-visualizer` 文件夹复制到 skills 目录：

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse -Path "f:\##cfmy-2025\graf\cc-code\graphviz-visualizer" -Destination "$env:USERPROFILE\.claude\skills\"
```

**macOS/Linux:**
```bash
cp -r /path/to/graphviz-visualizer ~/.claude/skills/
```

**或者使用图形界面直接拖拽复制**

### 步骤 3: 重启 Claude Code

1. 完全关闭 Claude Code
2. 重新启动
3. 技能会自动加载

## 验证安装

在 Claude Code 中测试：

```
你: "列出可用的 skills"
或
你: "请帮我可视化一个简单的链表结构"
```

如果安装成功，Claude 会自动使用 `graphviz-visualizer` 技能并生成标准格式的 DOT 代码。

## 最终目录结构

安装完成后，你的目录应该是这样的：

```
~/.claude/skills/                              (或 %USERPROFILE%\.claude\skills\)
└── graphviz-visualizer/                       ← Skill 根目录
    ├── SKILL.md                               ← 核心技能定义（必需）
    ├── README.md                              ← 使用说明
    ├── INSTALLATION.md                        ← 本安装指南
    ├── references/                            ← 参考文档目录
    │   ├── style_guide.md                    ← 完整样式指南 (450+ 行)
    │   ├── color_scheme.md                   ← 颜色方案详解
    │   └── examples.md                       ← 9 种常见结构示例
    └── assets/                                ← 资源文件目录
        └── templates/                         ← DOT 模板目录
            ├── basic_template.dot            ← 基础结构模板
            ├── hash_table_template.dot       ← 哈希表模板
            └── dual_hash_template.dot        ← 双哈希表模板
```

## 文件说明

| 文件 | 必需？ | 大小 | 说明 |
|------|--------|------|------|
| `SKILL.md` | ✅ 必需 | ~8 KB | 核心技能定义，Claude 自动加载 |
| `README.md` | ⭐ 推荐 | ~12 KB | 完整使用说明和示例 |
| `references/style_guide.md` | ⭐ 推荐 | ~25 KB | 原始完整规范文档 |
| `references/color_scheme.md` | ⭐ 推荐 | ~8 KB | 颜色使用详解 |
| `references/examples.md` | ⭐ 推荐 | ~15 KB | 9 种可复用示例 |
| `assets/templates/*.dot` | 可选 | ~6 KB | 可直接复制的模板 |

**最小安装**: 仅需 `SKILL.md` 即可工作（约 8 KB）
**完整安装**: 全部文件约 74 KB，包含完整文档和示例

## 工作原理

### 1. 技能加载
- Claude Code 启动时扫描 `~/.claude/skills/` 目录
- 读取每个 skill 的 `SKILL.md` 文件头部的 YAML frontmatter
- 提取 `name` 和 `description` 字段

### 2. 自动激活
当用户请求匹配 `description` 中的关键词时：
- "Graphviz"
- "data structure visualization"
- "hash table"
- "DOT code"
- 等等

Claude 会自动加载完整的 `SKILL.md` 内容，并按照其中的指示工作。

### 3. 按需加载参考
- `references/` 和 `assets/` 中的文件不会立即加载
- Claude 会在需要时读取（如需要查看示例或模板）
- 这避免了占用过多的上下文窗口

## 高级配置

### 自定义技能名称

编辑 `SKILL.md` 的 frontmatter：
```yaml
---
name: "my-custom-graphviz"  # 修改这里
description: "你的自定义描述"
---
```

### 添加更多模板

在 `assets/templates/` 中添加新的 `.dot` 文件：
```bash
cd ~/.claude/skills/graphviz-visualizer/assets/templates/
# 添加你的模板文件
cp your_template.dot ./
```

### 扩展颜色方案

编辑 `references/color_scheme.md`，添加新的颜色定义。

## 卸载

如果需要移除这个技能：

```bash
# Windows (PowerShell)
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\graphviz-visualizer"

# macOS/Linux
rm -rf ~/.claude/skills/graphviz-visualizer
```

然后重启 Claude Code。

## 常见问题

### Q: 技能没有被加载？
**A**: 检查：
1. 目录名称是否正确：`graphviz-visualizer`
2. `SKILL.md` 文件是否存在且有效
3. YAML frontmatter 格式是否正确（三个短横线包围）
4. 是否重启了 Claude Code

### Q: 如何确认技能已加载？
**A**:
1. 在 Claude Code 中询问："你有哪些 skills 可用？"
2. 或直接测试："请用 graphviz 可视化一个链表"

### Q: 可以同时安装多个 skills 吗？
**A**: 可以！每个 skill 一个独立目录：
```
~/.claude/skills/
├── graphviz-visualizer/
├── another-skill/
└── yet-another-skill/
```

### Q: Windows 用户如何找到 .claude 目录？
**A**:
1. 按 `Win + R` 打开运行
2. 输入 `%USERPROFILE%`
3. 查找 `.claude` 文件夹（可能是隐藏的）
4. 如果看不到，启用"显示隐藏文件"

### Q: 技能会占用多少上下文？
**A**:
- 启动时：仅加载 `name` 和 `description`（~100 tokens）
- 激活时：加载完整 `SKILL.md`（~4000 tokens）
- 按需：读取参考文档（每个 3000-8000 tokens）

## 更新技能

如果 skill 有新版本：
1. 备份旧版本（如果有自定义修改）
2. 删除旧的 `graphviz-visualizer` 目录
3. 复制新版本到 skills 目录
4. 重启 Claude Code

## 与其他工具集成

### VSCode Graphviz Preview
如果使用 VSCode：
1. 安装 "Graphviz Preview" 扩展
2. Claude 生成 DOT 代码后保存为 `.dot` 文件
3. 在 VSCode 中右键 → "Preview Graphviz"

### 在线预览
无需安装 Graphviz，直接使用在线工具：
- https://dreampuf.github.io/GraphvizOnline/
- http://www.webgraphviz.com/
- https://edotor.net/

## 技术支持

如有问题或建议：
1. 查看 [`README.md`](README.md) 详细文档
2. 参考 [`references/style_guide.md`](references/style_guide.md)
3. 检查 [`references/examples.md`](references/examples.md) 中的示例

---

**安装完成！** 现在你可以让 Claude Code 为你创建专业的数据结构可视化图表了。

试试这个命令：
```
"请帮我可视化一个哈希表，包含 4 个桶和链表解决冲突"
```
