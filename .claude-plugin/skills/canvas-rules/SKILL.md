---
name: canvas-rules
description: This skill should be used when Vibe Writing agents need to create, modify, or organize Canvas whiteboards. Triggers when adding cards to Canvas after iteration, organizing board structure, or creating connections between cards.
version: 0.6.1
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

## Additional Resources

For detailed Canvas operations:
- **`references/dual-link.md`** - Complete Canvas workflow and dual-link integration
- **`references/example-project.md`** - Working Canvas structure example

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
