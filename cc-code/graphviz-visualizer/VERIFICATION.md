# Graphviz Visualizer Skill - 验证检查清单

## 文件完整性检查

### ✅ 必需文件
- [x] `SKILL.md` - 核心技能定义
- [x] `README.md` - 使用说明
- [x] `INSTALLATION.md` - 安装指南

### ✅ 参考文档
- [x] `references/style_guide.md` - 完整规范（原始文档）
- [x] `references/color_scheme.md` - 颜色方案
- [x] `references/examples.md` - 示例代码

### ✅ 模板文件
- [x] `assets/templates/basic_template.dot` - 基础模板
- [x] `assets/templates/hash_table_template.dot` - 哈希表模板
- [x] `assets/templates/dual_hash_template.dot` - 双哈希表模板

## SKILL.md 格式检查

### ✅ YAML Frontmatter
```yaml
---
name: "graphviz-visualizer"
description: "Expert at creating Graphviz DOT visualizations..."
---
```
- [x] 以 `---` 开始和结束
- [x] 包含 `name` 字段
- [x] 包含 `description` 字段
- [x] 描述中包含触发关键词

### ✅ 内容结构
- [x] 核心能力说明
- [x] 工作流程
- [x] 标准颜色映射
- [x] HTML Table 节点模板
- [x] 连接线规范
- [x] 特殊结构处理
- [x] 质量检查清单
- [x] 渲染命令

## 安装前检查

### 环境检查
```bash
# Windows (PowerShell)
Test-Path "$env:USERPROFILE\.claude\skills"

# macOS/Linux
ls -la ~/.claude/skills/
```

期望结果：
- [ ] Claude Code 已安装
- [ ] `.claude/skills/` 目录存在（或可创建）

## 安装步骤

### 1. 复制文件
```bash
# Windows (PowerShell)
Copy-Item -Recurse -Path "f:\##cfmy-2025\graf\cc-code\graphviz-visualizer" -Destination "$env:USERPROFILE\.claude\skills\"

# macOS/Linux
cp -r "f:/##cfmy-2025/graf/cc-code/graphviz-visualizer" ~/.claude/skills/
```

### 2. 验证目录结构
```bash
# 检查必需文件是否存在
ls ~/.claude/skills/graphviz-visualizer/SKILL.md
ls ~/.claude/skills/graphviz-visualizer/references/
ls ~/.claude/skills/graphviz-visualizer/assets/templates/
```

期望结果：
- [ ] 所有文件已复制
- [ ] 目录结构完整
- [ ] 无错误消息

### 3. 重启 Claude Code
- [ ] 完全关闭 Claude Code
- [ ] 重新启动
- [ ] 等待加载完成

## 功能测试

### 测试 1: 技能可见性
**输入**: "你有哪些 skills 可用？"
**期望输出**: 应列出 `graphviz-visualizer` 或提到数据结构可视化能力

结果: [ ] 通过 / [ ] 失败

---

### 测试 2: 简单链表可视化
**输入**: "请帮我可视化一个简单的链表结构，包含 3 个节点"

**期望输出**:
- [ ] 生成 DOT 代码
- [ ] 使用 `rankdir=LR`
- [ ] 使用 HTML `<TABLE>` 标签
- [ ] 节点使用标准颜色（lightyellow, palegreen）
- [ ] 包含连接线标签
- [ ] 代码可直接运行

结果: [ ] 通过 / [ ] 失败

---

### 测试 3: 哈希表可视化
**输入**: "创建一个哈希表的 Graphviz 图表，包含水平的桶数组和链表节点"

**期望输出**:
- [ ] 桶数组水平排列（单行多列）
- [ ] 使用 PORT 属性
- [ ] 哈希连接使用蓝色粗线
- [ ] 链表连接使用绿色
- [ ] 颜色符合标准方案

结果: [ ] 通过 / [ ] 失败

---

### 测试 4: 从结构体定义生成
**输入**:
```
请为这个结构体创建可视化：
struct node {
    int data;
    struct node *next;
};
```

**期望输出**:
- [ ] 识别结构体字段
- [ ] 生成对应的表格行
- [ ] 为 `next` 指针添加 PORT
- [ ] 建议连接方式

结果: [ ] 通过 / [ ] 失败

---

### 测试 5: 自动应用颜色方案
**输入**: "可视化一个 NAT servermap，包含 internal 和 external 哈希表"

**期望输出**:
- [ ] Internal 使用 lightblue
- [ ] External 使用 lightcoral
- [ ] Mapping 节点使用 palegreen
- [ ] 双哈希连接使用不同颜色

结果: [ ] 通过 / [ ] 失败

---

## 质量检查

### 生成代码质量
使用任意测试生成的 DOT 代码：

```bash
# 保存生成的代码为 test.dot
dot -Tsvg test.dot -o test.svg
```

检查项：
- [ ] 无语法错误
- [ ] 成功生成 SVG
- [ ] 图表布局合理
- [ ] 文字清晰可读
- [ ] 颜色正确应用
- [ ] 连接线清晰

---

## 性能测试

### Token 消耗估算
- [ ] 简单请求 token 消耗 < 5000
- [ ] 复杂请求 token 消耗 < 10000
- [ ] 响应时间 < 10 秒

---

## 故障排查

### 问题 1: 技能未加载
可能原因：
- [ ] SKILL.md YAML 格式错误
- [ ] 文件路径不正确
- [ ] 未重启 Claude Code

解决方法：
1. 检查 YAML frontmatter 格式
2. 确认文件路径：`~/.claude/skills/graphviz-visualizer/SKILL.md`
3. 完全重启 Claude Code

---

### 问题 2: 生成的代码不符合规范
可能原因：
- [ ] 技能未被正确激活
- [ ] SKILL.md 内容不完整

解决方法：
1. 显式请求："使用 graphviz-visualizer skill"
2. 检查 SKILL.md 内容完整性

---

### 问题 3: 渲染失败
可能原因：
- [ ] Graphviz 未安装
- [ ] DOT 语法错误

解决方法：
1. 安装 Graphviz: `apt-get install graphviz` 或从官网下载
2. 使用在线工具测试：https://dreampuf.github.io/GraphvizOnline/

---

## 最终验证清单

### 安装验证
- [ ] 所有文件已复制到正确位置
- [ ] Claude Code 可以识别技能
- [ ] 技能可以被自动激活

### 功能验证
- [ ] 可以生成简单数据结构
- [ ] 可以生成复杂数据结构（哈希表等）
- [ ] 颜色方案正确应用
- [ ] 生成的代码可以渲染

### 质量验证
- [ ] 代码符合样式指南
- [ ] 图表专业美观
- [ ] 响应速度acceptable
- [ ] Token 消耗合理

---

## 验证结果

**日期**: ___________
**测试者**: ___________

**总体评分**: _____ / 5

**评价**:
- [ ] 完全满足要求，可以发布
- [ ] 基本满足要求，有小问题需修复
- [ ] 不满足要求，需要重大修改

**备注**:
___________________________________________
___________________________________________
___________________________________________

---

## 下一步

### 如果测试通过
1. [ ] 编写发布说明
2. [ ] 打包为 ZIP（可选）
3. [ ] 分享给团队或社区
4. [ ] 记录版本号和变更日志

### 如果测试未通过
1. [ ] 记录具体问题
2. [ ] 修复问题
3. [ ] 重新测试
4. [ ] 更新文档

---

**验证清单结束**

完整安装指南请参考: [INSTALLATION.md](INSTALLATION.md)
使用说明请参考: [README.md](README.md)
