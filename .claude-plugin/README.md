# Vibe Writing Plugin

**版本**：v0.8.1
**最后更新**：2025-12-28
**许可证**：MIT

---

## 📦 安装

### 方式一：通过 GitHub 克隆到插件目录

```bash
# Windows PowerShell
# 1. 克隆仓库
git clone https://github.com/gitboy808/vibe-writing.git

# 2. 复制到 Claude Code 插件目录
mkdir "$env:APPDATA\Claude\plugins\vibe-writing"
xcopy vibe-writing\.claude-plugin "$env:APPDATA\Claude\plugins\vibe-writing\.claude-plugin\" /E /I /H /Y

# 3. 重启 Claude Code
```

```bash
# macOS/Linux
git clone https://github.com/gitboy808/vibe-writing.git
mkdir -p ~/.claude/plugins/vibe-writing
cp -r vibe-writing/.claude-plugin ~/.claude/plugins/vibe-writing/
```

### 方式二：本地测试（开发模式）

使用 `--plugin-dir` 标志直接加载插件，无需安装。

```bash
# 克隆仓库
git clone https://github.com/gitboy808/vibe-writing.git
cd vibe-writing

# 使用插件目录启动 Claude Code
claude --plugin-dir .
```

### 方式三：通过 Marketplace 安装

如果你的 Claude Code 版本支持插件市场，直接使用：

```bash
/plugin install gitboy808/vibe-writing
```

---

## ⚙️ 配置

### 基础配置

无需任何配置即可使用！插件会自动：

- ✅ 检测工作目录
- ✅ 识别项目意图
- ✅ 加载对应的 Agent

### 可选配置

如果需要自定义某些行为，可以创建 `.claude-plugin/vibe-writing.local.md`：

```yaml
---
# 自定义项目目录名称（默认：项目）
project_dir: "我的项目"

# 自定义初始文档字数（默认：3000）
initial_doc_words: 5000
---
```

---

## 📖 系统简介

**Vibe Writing 是一个思维协同系统，不是AI写作助手。**

**核心理念**：以人为主体性——这是**你的主题**，我是你的协同伙伴。你掌握、你选择、你决定，我激发、总结、建议、润色。

### 四阶段工作流

1. **学习阶段**（learning-agent）：我和你对话，激发灵感，挖掘你的潜意识，总结成知识卡片
2. **结构阶段**（structure-agent）：我帮你看到隐藏的价值点，你来决定讲什么、怎么排布
3. **写作阶段**（writing-agent）：我润色，你把控。我们一起迭代优化
4. **成稿阶段**（draft-agent）：我串联所有内容，形成完整文章

---

## 🚀 使用方式

### 方式一：自然语言（推荐）

直接用自然语言告诉AI你想做什么：

| 输入 | 触发阶段 |
|------|----------|
| "我想写关于XX"、"探索XX" | 学习阶段 |
| "输出"、"结构化"、"分析下一步" | 结构阶段 |
| "迭代"、"润色"、"整理" | 写作阶段 |
| "成稿"、"撰写"、"最终输出" | 成稿阶段 |

**无需任何配置**，AI会自动识别意图并加载对应Agent。

### 方式二：快捷命令

| 命令 | 说明 |
|------|------|
| `/structure` | 深度分析下一步写什么（结构Agent） |
| `/iterate` | 进入迭代对话模式（写作Agent） |
| `/polish` | 直接润色当前卡片（写作Agent） |
| `/draft` | 生成最终成稿（成稿Agent） |
| `/generate-card` | 提前生成知识卡片（学习Agent） |

### 智能引导

AI 会在合适的时机引导你使用快捷命令：

- **新用户**：展示所有可用命令
- **学习完成时**：提示 `/structure`
- **首版生成后**：提示 `/iterate`
- **迭代完成后**：提示 `/polish`
- **成稿前**：提示 `/draft`

---

## 📁 项目结构

```
.claude-plugin/
├── plugin.json          # Plugin 配置（含元数据、仓库、许可证）
├── README.md            # 本文件
├── agents/              # 4个子Agent
│   ├── learning-agent.md    # 学习Agent（对话引导）
│   ├── structure-agent.md   # 结构Agent（价值点分析）
│   ├── writing-agent.md     # 写作Agent（迭代润色）
│   └── draft-agent.md       # 成稿Agent（串联成文）
├── commands/            # 5个快捷命令
│   ├── structure.md         # 结构分析命令
│   ├── iterate.md           # 迭代对话命令
│   ├── polish.md            # 润色命令
│   ├── draft.md             # 成稿命令
│   └── generate-card.md     # 卡片生成命令
├── skills/              # 4个核心技能
│   ├── vibe-workflow/SKILL.md  # 统筹入口（自然语言触发）
│   ├── core-specs/SKILL.md     # 核心规范
│   ├── format-check/SKILL.md   # 格式检查
│   └── canvas-rules/SKILL.md   # Canvas白板规则
└── references/          # 共享参考文档
    ├── README.md              # 参考文档说明
    └── example-project.md     # 完整示例项目
```

**组件统计**：
- 4个 Agents（工作流Agent）
- 5个 Commands（快捷命令）
- 4个 Skills（核心技能）
- 2个 References（共享文档）

---

## 🎯 使用流程

### 1. 启动新项目

直接告诉AI你想写什么：
```
"我想写关于XX"
"探索XX"
"开始一个新项目"
```

**无需任何配置**，AI会自动创建项目结构、生成初始文档、引导进入对话。

### 2. 学习对话

每4轮对话自动生成一张知识卡片，或随时说"生成卡片"提前生成。

### 3. 结构输出

当知识卡片积累到一定程度，说"输出"或使用 `/structure` 分析下一步写什么。

### 4. 迭代优化

- 说"迭代"、"润色"进入对话迭代模式
- 或使用 `/iterate`、`/polish`

### 5. 最终成稿

当所有输出卡片完成后，说"成稿"或使用 `/draft` 生成完整文章。

---

## 📚 示例项目

查看 `references/example-project.md` 了解完整工作流示例（Claude和GPT的区别）。

---

## 🔧 核心原则

1. **AI永远不做"决定"，只做"执行"和"建议"**
2. **人掌握知识、人掌握结构、人掌握内容**
3. **最终文章是"你的"，不是AI代写的**
4. **即装即用**：无需配置CLAUDE.md，自然语言直接触发

---

## ❓ 常见问题（FAQ）

### Q1：插件会自动修改我的文件吗？

**A**：不会。插件只在以下情况会创建/修改文件：
- 你明确说"开始新项目"时创建项目结构
- 你说"生成卡片"、"迭代"、"润色"等明确指令时
- AI 会先说明即将执行的操作，获得你的确认

### Q2：支持哪些语言？

**A**：主要用于中文写作，但核心逻辑可以适配任何语言。

### Q3：项目文件保存在哪里？

**A**：默认保存在 `项目/[项目名]/` 目录。你可以在 `.local.md` 中自定义。

### Q4：可以同时进行多个写作项目吗？

**A**：可以。每个项目都是独立的，互不干扰。

### Q5：如何删除一个项目？

**A**：直接删除 `项目/[项目名]/` 目录即可。

### Q6：插件会联网吗？

**A**：仅在生成初始文档时会使用 WebSearch 查询最新信息，其他时间不联网。

---

## 🤝 贡献

欢迎贡献！请遵循以下流程：

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

**贡献指南**：
- 遵循现有代码风格
- 添加必要的文档
- 测试你的更改
- 确保所有 Agents 的工具调用明确（使用 Bash、Read、Write、Edit）

---

## 📮 反馈与支持

- **问题报告**：[GitHub Issues](https://github.com/gitboy808/vibe-writing/issues)
- **功能建议**：[GitHub Discussions](https://github.com/gitboy808/vibe-writing/discussions)
- **使用问题**：查看 [FAQ](#-常见问题faq) 或提交 Issue

---

## 🗺️ 路线图

- [x] v0.7.0 - 自然语言触发机制
- [ ] v0.8.0 - 支持多语言
- [ ] v0.9.0 - 导出为 PDF/Word
- [ ] v1.0.0 - 协作编辑功能

---

## 📄 许可证

MIT License - 详见 [LICENSE](https://github.com/gitboy808/vibe-writing/blob/main/LICENSE)

---

## 🙏 致谢

感谢所有为 Vibe Writing System 提供反馈和建议的用户！

特别感谢：
- Anthropic 团队的 Claude Code 平台
- 所有测试用户的宝贵意见
