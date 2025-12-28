# Vibe Writing System

**版本**: v0.8.1 | **Claude Code Plugin**

以人为主体的思维协同写作系统 - 让你掌握知识、掌握结构、掌握内容，最终文章是"你的"，不是AI代写的。

**v0.8.0 更新**：核心规范内联，支持全局安装，完全消除路径依赖。

---

## ✨ 特性

### 四阶段工作流

```
学习阶段 → 结构阶段 → 写作阶段 → 成稿阶段
```

| 阶段 | Agent | 说明 |
|------|-------|------|
| **学习** | learning-agent | 对话激发灵感，生成知识卡片 |
| **结构** | structure-agent | 价值点分析，生成首版输出卡片 |
| **写作** | writing-agent | 迭代优化，润色，白板组织 |
| **成稿** | draft-agent | 串联输出卡片，生成完整文章 |

### 快捷命令

| 命令 | 功能 |
|------|------|
| `/structure` | 深度分析下一步写什么 |
| `/iterate` | 进入迭代对话模式 |
| `/polish` | 直接润色当前卡片 |
| `/draft` | 生成最终成稿 |
| `/generate-card` | 提前生成知识卡片 |

---

## 🚀 安装

### 方式1：本地测试（开发模式）

使用 `--plugin-dir` 标志直接加载插件，无需安装。

```bash
# 克隆仓库
git clone https://github.com/gitboy808/vibe-writing.git
cd vibe-writing

# 使用插件目录启动 Claude Code
claude --plugin-dir .
```

**说明**：
- 无需安装，直接从插件目录加载
- 适合开发和测试
- 每次修改插件后需要重启 Claude Code

---

### 方式2：手动安装到全局目录

将插件复制到 Claude Code 的插件目录。

#### Windows

```powershell
# 1. 克隆仓库
git clone https://github.com/gitboy808/vibe-writing.git

# 2. 复制到插件目录
# 创建插件目录（如果不存在）
mkdir "$env:APPDATA\Claude\plugins\vibe-writing"

# 复制插件内容
xcopy vibe-writing\.claude-plugin "$env:APPDATA\Claude\plugins\vibe-writing\.claude-plugin\" /E /I /H /Y
```

#### macOS/Linux

```bash
# 1. 克隆仓库
git clone https://github.com/gitboy808/vibe-writing.git

# 2. 复制到插件目录
mkdir -p ~/.claude/plugins/vibe-writing
cp -r vibe-writing/.claude-plugin ~/.claude/plugins/vibe-writing/
```

**说明**：
- 插件安装在用户级别的全局目录
- 所有项目都可以使用此插件
- 安装后需要重启 Claude Code

---

### 方式3：从 Marketplace 安装

如果你的 Claude Code 版本支持插件市场，直接使用：

---

### ✅ 验证安装

在 Claude Code 中执行：

```bash
/plugin list
```

应该看到：

```
vibe-writing (v0.8.1)
   ├── 4 agents
   ├── 5 commands
   └── 5 skills
```

---

## 📖 快速开始

### 1. 启动新项目

直接告诉 AI 你想写什么：

```
"我想写关于AI为什么能秒懂"
"探索量子计算的原理"
```

学习Agent 会自动：
- 创建项目结构
- 生成初始研究文档
- 引导对话探索

### 2. 学习对话

每4轮对话自动生成知识卡片，或随时说 `/generate-card` 提前生成。

### 3. 结构输出

使用 `/structure` 分析下一步写什么，生成首版输出卡片。

### 4. 迭代优化

- `/iterate` - 对话迭代模式
- `/polish` - 直接润色语言

### 5. 最终成稿

使用 `/draft` 生成完整文章。

---

## 🎯 核心原则

### AI永远不做"决定"

- **是什么** → 人决定
- **怎么说** → AI优化
- **为什么** → 人想清楚

### 你掌握一切

- 你掌握知识
- 你掌握结构
- 你掌握内容

### 最终文章是"你的"

不是AI代写，是你通过对话激发思考后，AI帮你整理和优化的结果。

---

## 📂 项目结构

```
vibe-writing/
├── .claude-plugin/           # Claude Code Plugin 目录
│   ├── plugin.json           # Plugin 配置（元数据、仓库、许可证）
│   ├── README.md             # Plugin 详细说明
│   ├── agents/               # 4个工作流Agent
│   │   ├── learning-agent.md    # 学习Agent（对话引导）
│   │   ├── structure-agent.md   # 结构Agent（价值点分析）
│   │   ├── writing-agent.md     # 写作Agent（迭代润色）
│   │   └── draft-agent.md       # 成稿Agent（串联成文）
│   ├── commands/             # 5个快捷命令
│   │   ├── structure.md         # 结构分析命令
│   │   ├── iterate.md           # 迭代对话命令
│   │   ├── polish.md            # 润色命令
│   │   ├── draft.md             # 成稿命令
│   │   └── generate-card.md     # 卡片生成命令
│   ├── skills/               # 4个核心技能
│   │   ├── vibe-workflow/SKILL.md  # 统筹入口
│   │   ├── core-specs/SKILL.md     # 核心规范
│   │   ├── format-check/SKILL.md   # 格式检查
│   │   └── canvas-rules/SKILL.md   # Canvas白板规则
│   └── references/           # 共享参考文档
│       ├── README.md              # 参考文档说明
│       └── example-project.md     # 完整示例项目
├── CLAUDE.md                  # 系统入口文档
└── README.md                  # 本文档（项目首页）
```

**组件统计**：
- 4个 Agents（工作流Agent）
- 5个 Commands（快捷命令）
- 4个 Skills（核心技能）

---

## 💡 工作流演示

```
用户: "我想写关于AI为什么能秒懂"

AI: [创建项目、生成初始文档]
    "请先阅读初始文档，读完后告诉我你的第一个问题"

用户: "Transformer是怎么理解上下文的？"

AI: [深入回答 + 引导问题]

...（4轮对话后）

AI: [自动生成知识卡片]

用户: "/structure"

AI: [分析选题，提供方案]

用户: "选择方案A"

AI: [生成首版输出卡片]

用户: "/iterate"

AI: [进入对话迭代]

...

用户: "/draft"

AI: [生成最终成稿]
```

---

## 📦 Marketplace 指南（插件作者）

### 什么是 Marketplace？

**Marketplace** 是一个包含**多个插件**的 GitHub 仓库，用户可以通过 `/plugin marketplace add` 命令添加，然后使用 `/plugin` 命令浏览和安装插件。

### 创建 Marketplace 仓库

**步骤1：创建新仓库**

```bash
# 在 GitHub 上创建新仓库：gitboy808/claude-plugins
# 然后克隆到本地
git clone https://github.com/gitboy808/claude-plugins.git
cd claude-plugins
```

**步骤2：创建目录结构**

```bash
mkdir -p plugins/vibe-writing
```

**步骤3：复制插件内容**

```bash
# 方式A：直接复制
cp -r ../vibe-writing/.claude-plugin plugins/vibe-writing/

# 方式B：使用 git submodule（推荐）
git submodule add https://github.com/gitboy808/vibe-writing.git plugins/vibe-writing
```

**步骤4：创建 marketplace.json**

```json
{
  "name": "claude-plugins",
  "description": "Claude Code Plugins Marketplace - 社区驱动的 Claude Code 插件集合",
  "version": "1.0.0",
  "author": {
    "name": "gitboy808",
    "url": "https://github.com/gitboy808"
  },
  "homepage": "https://github.com/gitboy808/claude-plugins",
  "repository": {
    "type": "git",
    "url": "https://github.com/gitboy808/claude-plugins.git"
  },
  "plugins": [
    {
      "name": "vibe-writing",
      "description": "以人为主体的思维协同写作系统",
      "version": "0.8.1",
      "path": "plugins/vibe-writing/.claude-plugin",
      "homepage": "https://github.com/gitboy808/vibe-writing",
      "categories": ["productivity", "writing", "knowledge-management"]
    }
  ]
}
```

**步骤5：提交并推送**

```bash
git add .
git commit -m "feat: 初始化 marketplace，添加 vibe-writing 插件"
git push origin main
```

### 目录结构要求

```
claude-plugins/                    # Marketplace 仓库根目录
├── marketplace.json              # Marketplace 配置
├── README.md                      # Marketplace 说明
└── plugins/                       # 插件目录
    └── vibe-writing/              # 单个插件子目录
        └── .claude-plugin/        # 插件配置目录
            ├── plugin.json        # 插件元数据
            ├── agents/
            ├── commands/
            ├── skills/
            └── ...
```

---

## ❓ 故障排除

### 插件没有加载？

**检查清单**：
1. ✅ 确认 Claude Code 版本 ≥ 1.0.33
2. ✅ 确认安装方式正确：
   - **方式1**：使用 `--plugin-dir` 启动
   - **方式2**：插件已复制到 `~/.claude/plugins/` 或 `%APPDATA%\Claude\plugins\`
   - **方式3**：通过 marketplace 安装
3. ✅ 确认 `plugin.json` 格式正确（无语法错误）
4. ✅ 重启 Claude Code 使插件生效

### 命令不工作？

**可能原因**：
- ❌ 使用了错误的命令格式
- ✅ 正确：`/structure`、`/iterate`
- ❌ 错误：`\structure`、`structure/`

### `/plugin install` 不工作？

**原因**：`/plugin install` 命令只适用于 **marketplace 仓库**，不适用于单个插件仓库。

**解决**：
- 使用手动安装方式（方式1）
- 或创建 marketplace 仓库（见上方 Marketplace 指南）

### Agent 没有自动触发？

**确认触发词**：
- ✅ "我想写关于XX" → learning-agent
- ✅ "输出"、"结构化" → structure-agent
- ✅ "迭代"、"润色" → writing-agent
- ✅ "成稿"、"撰写" → draft-agent

如果仍然不工作，尝试使用**快捷命令**（如 `/structure`）。

---

## 📚 更多文档

- **插件详细说明**：查看 `.claude-plugin/README.md`
- **开发文档**：查看 `CLAUDE.md`
- **示例项目**：查看 `.claude-plugin/references/example-project.md`

---

## 🔗 相关链接

- **GitHub 仓库**：[https://github.com/gitboy808/vibe-writing](https://github.com/gitboy808/vibe-writing)
- **问题反馈**：[GitHub Issues](https://github.com/gitboy808/vibe-writing/issues)
- **功能建议**：[GitHub Discussions](https://github.com/gitboy808/vibe-writing/discussions)
- **官方文档**：[Claude Code Plugins README](https://github.com/anthropics/claude-code/blob/main/plugins/README.md)
- **官方 Marketplace**：[anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📄 许可证

MIT License

---

## 🙏 致谢

感谢 Anthropic 团队开发出如此强大的 Claude Code 平台！
