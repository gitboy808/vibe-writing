# Vibe Writing Plugin

**版本**：v0.7.0
**最后更新**：2025-12-27

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
├── plugin.json          # Plugin配置
├── marketplace.json     # 市场配置
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
└── references/          # 参考文档
    ├── README.md            # 参考文档说明
    ├── dual-link.md         # 双链系统
    ├── knowledge-card.md    # 知识卡片组织
    ├── output-card.md       # 输出卡片组织
    ├── checkpoint.md        # 断点续传
    └── example-project.md   # 完整示例项目
```

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

## 📄 许可证

MIT License
