# Graphviz 数据结构可视化规范 - Claude Code Skill 打包说明

## 文档信息
- **创建日期**: 2025-11-10
- **原始规范**: `cc-doc/graphviz_data_structure_style_guide.md`
- **Skill 输出**: `cc-code/graphviz-visualizer/`
- **参考项目**: https://github.com/yusufkaraaslan/Skill_Seekers

## 项目背景

根据 GitHub 项目 Skill_Seekers 的规范，将已有的 Graphviz 数据结构可视化通用规则文档打包成 Claude Code 可用的 skill 技能包。这样可以在未来的项目中直接通过 skill 方式调用这套标准化的可视化规范。

## Skill 结构设计

### 1. 核心文件
```
graphviz-visualizer/
├── SKILL.md                    # 核心技能定义（必需）
├── README.md                   # 完整使用说明
├── INSTALLATION.md             # 安装指南
├── references/                 # 参考文档（按需加载）
│   ├── style_guide.md         # 原始完整规范（450+ 行）
│   ├── color_scheme.md        # 颜色方案详解
│   └── examples.md            # 9 种常见结构示例
└── assets/                     # 资源文件
    └── templates/              # 可复用 DOT 模板
        ├── basic_template.dot
        ├── hash_table_template.dot
        └── dual_hash_template.dot
```

### 2. 文件功能说明

| 文件 | 大小 | 作用 | 加载时机 |
|------|------|------|----------|
| **SKILL.md** | ~8 KB | 核心技能定义、工作流程、快速参考 | 自动加载 |
| **README.md** | ~12 KB | 完整使用说明、示例、FAQ | 用户查阅 |
| **INSTALLATION.md** | ~10 KB | 安装步骤、目录结构、故障排查 | 安装时查阅 |
| **style_guide.md** | ~25 KB | 原始完整规范文档（未修改） | 按需参考 |
| **color_scheme.md** | ~8 KB | 颜色使用详细说明和决策树 | 按需参考 |
| **examples.md** | ~15 KB | 9 种数据结构的完整示例代码 | 按需参考 |
| **templates/*.dot** | ~2 KB/个 | 可直接复制修改的模板 | 按需复制 |

**总大小**: 约 74 KB（最小安装仅需 SKILL.md ~8 KB）

## SKILL.md 设计要点

### 1. YAML Frontmatter（元数据）
```yaml
---
name: "graphviz-visualizer"
description: "Expert at creating Graphviz DOT visualizations for complex data structures (kernel data structures, hash tables, linked lists, network protocols) using standardized HTML table-based node formatting and consistent styling. Use when users request data structure visualization, Graphviz diagrams, or converting code structures to visual representations."
---
```

**关键字段**：
- `name`: 技能唯一标识符
- `description`: 触发条件描述，包含关键词：
  - "Graphviz DOT visualizations"
  - "data structures"
  - "HTML table-based formatting"
  - "visualization"

### 2. 内容结构（Markdown 正文）

#### 核心能力（What）
明确说明技能适用场景：
- 内核数据结构
- 网络协议栈
- 哈希表、链表等
- 结构体转换为可视化

#### 工作流程（How）
5 步标准流程：
1. 理解需求
2. 规划图表结构
3. 生成 DOT 代码
4. 代码组织
5. 输出格式

#### 快速参考（Quick Reference）
- 标准颜色映射表
- HTML Table 节点模板
- 连接线规范
- 特殊结构处理模板

#### 质量标准（Quality）
- 检查清单
- 最佳实践
- 常见错误避免

#### 输出示例（Examples）
- 完整 DOT 代码结构
- 渲染命令
- 在线工具链接

## 技能激活机制

### 自动激活触发词
用户说以下内容时自动激活：
- "可视化 [数据结构]"
- "创建 Graphviz 图表"
- "生成 DOT 代码"
- "画一个哈希表"
- "为这个结构体生成图表"

### 手动调用
```
用户: "使用 graphviz-visualizer skill 帮我..."
```

## 与原始规范的关系

### 原始文档（style_guide.md）
- 完整的 450+ 行技术规范
- 包含所有细节和边缘情况
- 作为权威参考保留在 `references/` 中

### SKILL.md（精简版）
- 提取核心工作流程和决策点
- 快速参考表格和模板
- 保持在 ~300 行以减少 token 消耗
- 通过引用指向完整文档

### 设计理念
**渐进式加载（Progressive Disclosure）**：
```
启动 → 仅加载 name/description (~100 tokens)
  ↓
匹配 → 加载完整 SKILL.md (~4000 tokens)
  ↓
需要细节 → 读取 references/* (~3000-8000 tokens/文件)
```

## 安装方法

### 标准安装路径
```bash
# Windows
%USERPROFILE%\.claude\skills\graphviz-visualizer\

# macOS/Linux
~/.claude/skills/graphviz-visualizer/
```

### 安装步骤
1. 复制整个 `graphviz-visualizer` 目录到 skills 目录
2. 重启 Claude Code
3. 验证：询问 "你有哪些 skills？"

## 使用示例

### 示例 1: 直接可视化
```
用户: "请为这个哈希表结构创建可视化图表"
Claude: [自动使用 graphviz-visualizer skill]
       [生成标准格式的 DOT 代码]
       [包含颜色、端口、连接线等]
```

### 示例 2: 从结构体定义
```
用户: "这是我的结构体定义：
struct hash_node {
    char *key;
    int value;
    struct hash_node *next;
};
请帮我可视化"

Claude: [分析结构]
       [生成包含所有字段的节点]
       [添加 next 指针连接]
```

### 示例 3: 改进现有图表
```
用户: "这是我之前的 DOT 代码，请按照标准规范改进"
Claude: [读取现有代码]
       [应用标准颜色方案]
       [改为 HTML table 格式]
       [添加清晰标签]
```

## 技术特点

### 1. 标准化输出
✅ 统一的 HTML table 节点格式
✅ 一致的颜色语义
✅ 规范的连接线样式
✅ 清晰的层次结构

### 2. 高可复用性
✅ 丰富的示例库（9 种常见结构）
✅ 可直接复制的模板
✅ 模块化的文档结构
✅ 易于扩展和定制

### 3. 智能引导
✅ 自动选择合适的布局方向
✅ 智能分配颜色
✅ 提供渲染命令
✅ 包含在线工具链接

### 4. 文档完备
✅ 完整的安装指南
✅ 详细的使用说明
✅ 丰富的 FAQ
✅ 故障排查步骤

## 扩展性设计

### 添加新的数据结构模板
1. 在 `references/examples.md` 中添加示例
2. 如需独立模板，添加到 `assets/templates/`
3. 更新 `README.md` 的示例列表

### 自定义颜色方案
1. 编辑 `references/color_scheme.md`
2. 更新 `SKILL.md` 中的颜色表
3. 在模板中应用新颜色

### 支持新的布局类型
1. 在 `style_guide.md` 中添加新章节
2. 在 `examples.md` 中提供示例
3. 更新 `SKILL.md` 的特殊结构处理部分

## 对比其他方案

### 方案 1: 直接使用原始文档
❌ 450+ 行文档太长，占用大量 token
❌ 缺乏工作流程指导
❌ 没有快速参考

### 方案 2: 完全精简版本
❌ 丢失重要细节
❌ 缺乏权威参考
❌ 难以处理复杂情况

### 方案 3: 本 Skill 方案（采用）
✅ 核心文件精简（~8 KB）
✅ 保留完整参考（按需加载）
✅ 提供快速参考和模板
✅ 渐进式信息披露
✅ 易于安装和使用

## 性能考虑

### Token 消耗
- **启动阶段**: ~100 tokens（仅元数据）
- **激活阶段**: ~4,000 tokens（SKILL.md）
- **深入参考**: +3,000-8,000 tokens/文件（可选）

### 优化策略
1. **压缩重复信息**: 用表格替代冗长描述
2. **模块化文档**: 按需加载详细参考
3. **快速参考卡片**: 在 SKILL.md 中提供核心信息
4. **模板文件**: 外部 `.dot` 文件，不占用主文件

## 维护和更新

### 版本控制
当前版本：v1.0 (2025-11-10)

### 更新流程
1. 修改相应的 Markdown 文件
2. 更新 `README.md` 的版本历史
3. 如有重大变更，更新 YAML frontmatter 的 description

### 向后兼容
- SKILL.md 的 YAML 格式保持稳定
- 新功能通过新增章节而非修改现有结构
- 旧模板继续有效

## 实际应用价值

### 1. 代码文档化
快速为代码库生成数据结构文档

### 2. 系统设计
可视化系统架构和数据流

### 3. 教学材料
生成清晰的教学图表

### 4. 技术博客
创建专业的配图

### 5. Code Review
直观展示复杂的数据关系

## 成功标准

### 安装成功
✅ 用户可以在 Claude Code 中看到 graphviz-visualizer skill
✅ 自动激活机制正常工作

### 输出质量
✅ 生成的 DOT 代码符合规范
✅ 可以直接通过 `dot` 命令渲染
✅ 视觉风格一致且专业

### 用户体验
✅ 无需记忆复杂语法
✅ 自然语言交互
✅ 快速获得高质量输出

## 总结

本 skill 成功将 450+ 行的完整技术规范转换为可部署的 Claude Code 技能包，实现了：

1. **标准化**: 统一的可视化风格和规范
2. **自动化**: 自然语言描述即可生成专业图表
3. **可扩展**: 易于添加新模板和自定义
4. **高性能**: 渐进式加载，优化 token 使用
5. **文档完备**: 从安装到使用的全流程指导

**应用场景**: 内核开发、系统设计、技术文档、教学材料等需要数据结构可视化的场景。

**技术栈**:
- Claude Code Skills 框架
- Graphviz DOT 语言
- HTML-like Labels
- Markdown 文档

**核心价值**: 让复杂的数据结构可视化变得简单、标准、高效。

---

## 附录：文件清单

### 已创建的文件
```
cc-code/graphviz-visualizer/
├── SKILL.md                              (8 KB)  ✅ 已创建
├── README.md                             (12 KB) ✅ 已创建
├── INSTALLATION.md                       (10 KB) ✅ 已创建
├── references/
│   ├── style_guide.md                   (25 KB) ✅ 已复制
│   ├── color_scheme.md                  (8 KB)  ✅ 已创建
│   └── examples.md                      (15 KB) ✅ 已创建
└── assets/
    └── templates/
        ├── basic_template.dot           (2 KB)  ✅ 已创建
        ├── hash_table_template.dot      (2 KB)  ✅ 已创建
        └── dual_hash_template.dot       (3 KB)  ✅ 已创建
```

### 本说明文档
```
cc-doc/graphviz_skill_packaging_guide.md    ✅ 当前文件
```

**总计**: 10 个文件，约 85 KB

---

**文档结束**

如需进一步定制或有疑问，请参考 `cc-code/graphviz-visualizer/README.md`
