---
name: canvas-rules
description: Standards for Obsidian Canvas operations. Covers node creation, layout, connections, and file path rules.
---

# Canvas Rules - Vibe Writing System

This skill defines the standards for working with Obsidian Canvas in the Vibe Writing System. Covers Canvas structure, node creation, connection design, and layout principles.

## Purpose

Establish consistent Canvas operations for:
- Creating and validating Canvas files
- Adding output card nodes automatically
- Organizing cards with logical connections
- Designing effective connection labels

## When to Use

Reference this skill when:
- Creating new Canvas files (`内容白板.canvas`)
- Adding nodes after iteration completion
- Organizing existing Canvas structure
- Creating connections between cards

## Critical Path Format Rules

### File Path Must Start with Working Directory Name

**Most Important Rule**: Canvas `file` paths must start with the working directory name obtained via `pwd`.

**AI Execution Steps**:
1. Execute `pwd` → Get full path (e.g., `/Users/.../vibe-writing-Tasihi`)
2. Extract last directory name → `vibe-writing-Tasihi` (save as `{WORK_DIR}`)
3. Use Glob to read actual paths, ensuring correct Chinese punctuation
4. Construct Canvas path: `{WORK_DIR}/项目/...`

**Examples**:
- ✅ Correct: `vibe-writing-Tasihi/项目/AI为什么能秒懂/输出卡片/...`
- ❌ Incorrect: `项目/...` (missing working directory prefix)
- ❌ Incorrect: Using English punctuation instead of Chinese

### JSON String Escaping

**Critical**: Special characters in file paths must be escaped in JSON:

- Double quote `"` → `\"`
- Backslash `\` → `\\`

**Example**:
- Original: `02-三个技术维度看"优势反转".md`
- Escaped: `02-三个技术维度看\"优势反转\".md`

**Why**: Unescaped quotes break Canvas JSON parsing.

## Node Standards

### Text Content Rules

- **Prohibited**: Chinese double quotes `""` (causes Canvas open failure)
- **Use instead**: `「」` for all Canvas text content

### Node Height

Set based on file line count:
- 20-30 lines → 600px height
- 40-50 lines → 900px height
- 50+ lines → 1100px height

### Node Type

Output cards use `type: "file"` with `color: "2"` for visual distinction.

## Connection Design Principles

### Core Rules

- **Use question format** (not statements)
- **Questions must be complete** with context (let readers understand what's being asked)
- **Don't repeat node content** (nodes already said it, connections shouldn't repeat)

### Examples

❌ **Incorrect** (lacks context):
```
"为什么要计算两次？"
```

✅ **Correct** (has context):
```
"预训练和推理都计算关系，为什么要计算两次？"
```

### Label Format

Use `「」` for connection labels:
```json
"label": "「预训练和推理都计算关系，为什么要计算两次？」"
```

## Vertical Layout Standards

### Layout Principle

All cards arranged vertically (`x=100` fixed, `y` increments).

### Coordinate Rules

- Card 1: `x=100, y=100`
- Card 2: `x=100, y=900` (800px gap)
- Card 3: `x=100, y=1700`
- Width: `800` (standard)
- Height: Calculated based on line count

### Connection Relationship

**Connected cards**:
- `fromSide: "bottom"`
- `toSide: "top"`
- `label` uses `「」`
- Complete question format

**Independent cards**: No connection lines initially

## Operation Workflows

### Workflow 1: Auto-Add After Iteration

**Trigger**: Writing-agent completes iteration

**Execution**:
1. Check if Canvas exists (create if not, Read if yes)
2. Use Glob to read current card's actual path (ensure correct punctuation)
3. Calculate coordinates: `x=100, y=100+(existing card count × 800)`
4. Create file node (`type: "file"`, `color: "2"`)
5. **JSON escape special characters** in path
6. Generate unique node ID (e.g., `node1`, `node2`...)
7. Write updated Canvas

**Path Example**:
```json
{
  "type": "file",
  "file": "vibe-writing-Tasihi/项目/AI为什么能秒懂/输出卡片/01-XX.md",
  "x": 100,
  "y": 1700,
  "width": 400,
  "height": 400,
  "color": "2",
  "id": "node3"
}
```

### Workflow 2: Manual Board Organization

**Trigger**: User says "组织白板", "建立连接", "这些卡片有什么关系"

**Execution**:
1. Execute `pwd` to get `{WORK_DIR}`
2. Read Canvas → Identify connected/independent cards
3. Analyze logical relationships → Identify logical chains
4. Provide 2-3 organization schemes
5. User confirms:
   - Vertical arrangement (`x=100`, `y` increments by 800)
   - Create connections (label with `「」`, JSON escaped)
   - Preserve user manual adjustments
6. Update `项目信息.md` "白板组织记录"
7. Write updated Canvas
8. **Verify JSON format** (optional but recommended)

## Node Organization Principles

### Content Logic > File Splitting

If multiple `.md` files describe one complete process → Merge `.md` files in file system first → Then create Canvas node

**Reason**: Canvas file nodes = `.md` files themselves (bidirectional sync)

## JSON Validation

After writing Canvas, optionally validate:

```bash
# Check if JSON is valid
python -m json.tool "项目/[项目名]/内容白板.canvas" > /dev/null && echo "Valid JSON" || echo "Invalid JSON"
```

**Common Issues**:
- Unescaped quotes in file paths
- Trailing commas in JSON arrays
- Invalid UTF-8 characters

## 🔗 Dual-Link System (内联规范)

### 本质

标记用户在哪里"停下来"产生疑问的历史记录。

### 操作流程

用户看文档 → 产生疑问 → 对话形成新卡片 → 把新卡片标题作为双链插入源文档

### 双链格式

`[[{WORK_DIR}/项目/[项目名]/知识卡片/[序号]. [标题]|[序号]. [标题]]]`

### {WORK_DIR} 自动获取

1. AI 生成双链前必须执行 `pwd`
2. 提取最后一级目录名（如 `vibe-writing-Tasihi`）
3. 动态构建完整双链路径

**示例**:
- 完整路径: `/Users/changcheng/Desktop/vibe-writing-Tasihi`
- 提取: `vibe-writing-Tasihi`
- 双链: `[[vibe-writing-Tasihi/项目/AI为什么能秒懂/知识卡片/01. xxx|01. xxx]]`

### 双链插入流程（6步）

**步骤1：判断来源**
AI 根据对话内容自主判断（不询问用户），优先级：
- 问题直接提及某文档/卡片 → 那个文档
- 问题延续上一轮话题 → 上一轮对应源
- 问题涉及初始文档概念 → 初始文档
- 无法判断 → 初始文档末尾

**步骤2：Read 源文档**

**步骤3：匹配插入位置**
理解对话逻辑，判断用户在哪里"停下来"产生疑问：
- 首次提问 → 插在对应内容位置
- 整体感受（"看完后"、"整体来说"）→ 插在末尾
- 综合疑问（涉及多处）→ 插在最核心位置

**步骤4：插入双链**
格式：在匹配位置后换行，单独一行，前后不空行，使用别名语法 `[[完整路径|序号. 标题]]`

**步骤5：Edit 保存**

**步骤6：处理多张卡片**
逐张处理：生成卡片1 → 判断来源 → Read → 匹配 → 插入 → 保存 → 生成卡片2 → 读取最新源文档 → 匹配 → 插入 → 保存

## Quick Reference

**When adding nodes to Canvas**:
1. ✅ Execute `pwd` → Get `{WORK_DIR}`
2. ✅ Use Glob to ensure correct Chinese punctuation
3. ✅ Escape special characters in JSON (`"` → `\"`, `\` → `\\`)
4. ✅ Set `type: "file"`, `color: "2"`
5. ✅ Calculate vertical layout (x=100, y=100+index×800)
6. ✅ Generate unique node ID

**When creating connections**:
1. ✅ Use question format with context
2. ✅ Wrap labels in `「」`
3. ✅ Set `fromSide: "bottom"`, `toSide: "top"`
4. ✅ JSON escape special characters in labels

**Common Errors**:
- ❌ Missing `{WORK_DIR}/` prefix in file paths
- ❌ Unescaped quotes in paths
- ❌ Using `""` instead of `「」` in labels
- ❌ Forgetting to validate JSON after edits
