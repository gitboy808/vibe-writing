---
name: core-specs
description: Reference for project metadata standards, file operations, and path conventions. Use when updating project info or validating structure.
version: 0.7.0
---

# Core Specifications - Vibe Writing System

This skill defines the core standards for Vibe Writing System: project metadata structure, file operation conventions, and path rules that all agents must follow.

## Purpose

Establish consistent standards across all Vibe Writing agents for:
- Project information metadata structure and update responsibilities
- File operation paths (relative vs absolute)
- Project directory structure
- File naming conventions

## When to Use

Reference this skill when:
- Creating or updating `项目信息.md`
- Performing file operations (Read/Write/Edit mkdir)
- Working with Obsidian features (Canvas, double-links)
- Validating project structure

## Path Conventions

### Current Working Directory

Always execute `pwd` at the start of any agent session to extract the working directory name (`{WORK_DIR}`).

**Path Rules:**
- **File operations** (mkdir, Read, Write, Edit): Use **relative paths** `项目/`, `_系统/`
- **Obsidian paths** (Canvas nodes, double-links): Use **full paths** `{WORK_DIR}/项目/...`

**Examples:**
- ✅ Correct: `mkdir -p "项目/[项目名]/知识卡片"`
- ❌ Incorrect: `mkdir -p "[项目名]/知识卡片"` (missing `项目/` prefix)
- Canvas node path: `{WORK_DIR}/项目/AI为什么能秒懂/输出卡片/01-xxx.md`
- Double-link format: `[[{WORK_DIR}/项目/.../知识卡片/01. xxx|01. xxx]]`

## Project Information Structure

The `项目信息.md` file contains 9 major fields. Each field is maintained by specific agents following strict responsibility separation.

### Field Overview

| Field | Maintained By | Update Timing |
|-------|--------------|---------------|
| Basic Info | learning-agent | Initialization only |
| Learning State | learning-agent | Every response + card generation |
| Output State | writing-agent | Start/end of optimization |
| Knowledge Card List | learning-agent | After card generation |
| Output Card List | structure-agent (add), writing-agent (update iterations) | After generation/optimization |
| Pending Conversion List | structure-agent | After conversion |
| Structure Analysis | structure-agent | After "analyze next" |
| Canvas Organization | writing-agent | After board organization |
| Final Draft | draft-agent | After draft generation |

### Core Principle

Each field is maintained by only one agent. No conflicts, no overlap.

## Project Structure

```
{WORK_DIR}/
├── _系统/           # System files
└── 项目/            # All projects
    └── [项目名]/
        ├── 项目信息.md
        ├── 初始文档.md
        ├── 知识卡片/
        └── 输出卡片/
            ├── [主题A]/
            │   ├── 01-XX.md
            │   └── 02-YY.md
            └── [主题B]/
```

## File Naming Conventions

- **Project name**: Extracted by AI from user's intent (concise, accurate)
- **Knowledge cards**: `[序号]. [问题式标题].md` (01, 02, 03...)
- **Output card nodes**: `01-[节点标题].md`

## Update Responsibilities

### Learning Agent Maintains

**字段B - 学习状态 (v0.1)**:
```markdown
## 📚 学习状态（v0.1）
**当前轮次**：第X轮
**距上次生成**：Y轮
**已生成知识卡片**：Z张
```

**字段D - 知识卡片列表**:
```markdown
## 📝 已生成的知识卡片

### 第X-Y轮生成
1. [[知识卡片/[标题]|[标题]]]
```

### Structure Agent Maintains

**字段E - 输出卡片列表** (adding new cards):
```markdown
## ✨ 输出卡片（优化后）
1. [[输出卡片/主题/01-节点.md|主题]] ← [[知识卡片/XX|XX]]
```

**字段F - 待转化列表** (removing converted cards):
```markdown
## 📋 待转化知识卡片
- [[知识卡片/[标题]|[标题]]]
```

**字段G - 结构分析记录**:
```markdown
## 📊 文章结构分析
**最近分析**（[日期]）：
- 已完成：[[输出卡片/主题/...|主题]]
- 选择方案：接下来写[X]，理由是[...]
```

### Writing Agent Maintains

**字段C - 输出状态 (v0.2+)**:
```markdown
## ✨ 输出状态（v0.2+）
**当前正在优化**：[路径] ← [[知识卡片]]（阶段说明） 或 无
```

**字段E - 输出卡片列表** (updating iteration count):
```markdown
## ✨ 输出卡片（优化后）
1. [[输出卡片/主题/01-节点.md|主题（迭代2次）]] ← [[知识卡片/XX|XX]]
```

**字段H - 白板组织记录**:
```markdown
## 🎨 白板组织记录
**最近组织**（[日期]）：
- 已添加连接：[[卡片A|主题A]] → [[卡片B|主题B]]
- 组织方式：纵向排列 + 逻辑连接
```

### Draft Agent Maintains

**字段I - 最终成稿**:
```markdown
## 📄 最终成稿

### 基本信息
**文件路径**：`项目/[项目名]/最终成稿-[项目名].md`
**生成时间**：[YYYY-MM-DD HH:mm]
**状态**：已完成 / 待润色 / 已完成最终润色

### 包含卡片
1. [[输出卡片/主题A/01-节点.md|主题A]]
2. [[输出卡片/主题B/01-节点.md|主题B]]
...
```

## 🔄 断点续传机制（内联）

### 🎯 场景判断

**"当前正在优化"有内容**:
- **识别**: 字段值：`主题/ ← [[知识卡片|标题]]（阶段说明）`
- **说明**: 上次正在优化某张输出卡片，但未完成
- **操作**:
  1. 识别阶段（第一阶段 vs 第二阶段）
  2. 提示用户上次进度
  3. 提供选项（见下方标准话术）
  4. 用户选择后 → 加载写作Agent

**"当前正在优化"为空或显示"无"**:
- **识别**: 字段值：`无`
- **说明**: 没有未完成的优化任务
- **操作**: 检查轮次 → 加载学习Agent，继续对话

### 📋 标准话术

**检测到未完成优化时**:
```
上次你正在优化《[卡片标题]》，已完成[阶段说明]。

你现在有两个选择：
1. **继续调整当前卡片**
2. **这张卡片完成了，分析下一步写什么**

你想怎么做？
```

**操作完成后的标准话术**:
```
已完成调整/润色，请查看。

你现在有两个选择：
1. **继续调整当前卡片**
   - 告诉我哪里需要修改（如"第2段太抽象"、"这个例子不好"）
   - 或你自己修改文件，改完后说"帮我润色"
2. **这张卡片完成了，分析下一步写什么**

你想怎么做？
```

**变体：初次生成输出卡片**:
```
我已经生成了优化版：
- 文件夹：输出卡片/[主题名]/
- 节点文件：01-XX.md, 02-YY.md, ...

你现在有两个选择：
1. **继续调整当前卡片**
2. **这张卡片完成了，分析下一步写什么**

你想怎么做？
```

### 🔑 核心原则

**绝不让用户不知道下一步**

每个对话节点都要有明确的出口：
- ✅ 选项要具体、可操作
- ✅ 包含"继续当前"和"进入下一步"两类选项
- ✅ 用问句结束，引导用户回应

## Quick Reference

**When updating project info:**
1. Read current `项目信息.md`
2. Identify which fields your agent maintains
3. Update only those fields using Edit tool
4. Never modify fields maintained by other agents

**When performing file operations:**
1. Start with `pwd` to get `{WORK_DIR}`
2. Use relative paths for Read/Write/Edit
3. Use full `{WORK_DIR}/...` paths for Canvas/double-links
4. Validate paths follow conventions
