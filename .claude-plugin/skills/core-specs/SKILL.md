---
name: core-specs
description: This skill should be used when the Vibe Writing agents need to reference project metadata standards, file operation conventions, or project information update rules. Triggers when agents need to update "项目信息.md", validate project structure, or understand path conventions (relative vs absolute paths for file operations vs Obsidian features).
version: 0.6.1
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

The `项目信息.md` file contains 7 major fields. Each field is maintained by specific agents following strict responsibility separation.

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

## Additional Resources

For detailed field structures and examples, consult:
- **`references/checkpoint.md`** - Complete field definitions with examples (断点续传机制)
- **`references/example-project.md`** - Real project info file example

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
