---
name: format-check
description: This skill should be used when Vibe Writing agents need to validate or correct knowledge card or output card file formats. Triggers when generating new cards, editing existing cards, or checking for formatting compliance (空行规则, 一级标题规则).
version: 0.6.1
---

# Format Check - Vibe Writing System

This skill defines file format standards for knowledge cards and output card nodes in the Vibe Writing System. All agents must follow these standards when generating or editing card files.

## Purpose

Ensure consistent file formatting across:
- Knowledge cards (知识卡片)
- Output card nodes (输出卡片节点)
- Prevent common formatting errors

## When to Use

Reference this skill when:
- Creating new knowledge cards or output nodes
- Editing existing card files
- Validating format compliance
- Troubleshooting formatting issues

## Knowledge Card Format Standards

### 空行规则 (Line Spacing Rules)

**Default Principle**: Never use blank lines

**Only 2 Exceptions**:
1. Before `## 二级标题` (level 2 headings) - add one blank line
2. Before the final ending引用块 (`>`) - add one blank line

**All other places - NO blank lines**:
- After `## 二级标题` - no blank line
- Before/after `### 三级标题` - no blank lines
- Between paragraphs - no blank lines
- Between text and lists - no blank lines
- Before/after double-links - no blank lines

**About Ending 引用块**:
Knowledge cards need an ending 引用块 to mark source (via double-link system):
```markdown
> [[源文档/知识卡片]]
```

### Example - Correct Format

```markdown
> [用户的疑问来源和思考过程]

## 第一章节
这是内容第一句。这是内容第二句。
### 小节
这是小节内容。
## 第二章节
这是第二章节内容。

> [[源文档/知识卡片]]
```

### Common Errors

❌ **Incorrect**: Blank lines after `##` heading
```markdown
## 第一章节

这是内容第一句。
```

✅ **Correct**: No blank lines
```markdown
## 第一章节
这是内容第一句。
```

## Output Card Node Format Standards

### ⚠️ No Level 1 Headings

Output card node files **must NOT have level 1 headings** (`#`), because Obsidian automatically uses the filename as the title.

- ❌ Incorrect: File starts with `# 标题`
- ✅ Correct: Content starts directly with `##` (level 2 heading)

### ⚠️ Absolutely No Blank Lines

Output card node files **must NOT have any blank lines**. All content must be tightly packed.

### ⚠️ No Ending 引用块 Needed

Output cards **do NOT need** to add the original knowledge card's 引用块 double-link at the end.

- ❌ Incorrect: Adding `> [[知识卡片/XX]]` at the end
- ✅ Correct: Content ends directly, no 引用块

### Example - Correct Format

```markdown
## 第一章节
这是内容第一句。这是内容第二句。
### 小节
这是小节内容。
## 第二章节
这是第二章节内容。
```

### Example - Incorrect Format

```markdown
# 节点标题
## 第一章节

这是内容第一句。

这是内容第二句。
```

## Format Validation Strategy

### Delayed Verification

**Principle**: Ensure format compliance during generation. Verify with Read tool during next access if issues found. No need for immediate Read-verification after Write.

**Approach**:
1. Generate content following format standards
2. Write to file
3. Proceed with next steps
4. If Read later reveals format issues, use Edit to correct

### Common Format Issues to Check

**Knowledge Cards**:
- ❌ `## 标题` has blank line after it
- ❌ `### 标题` has blank lines around it
- ❌ Missing ending `> [[source]]` 引用块

**Output Cards**:
- ❌ Any blank lines anywhere in the file
- ❌ Level 1 heading `#` at file start
- ❌ Unnecessary ending `> [[source]]` 引用块

## Format Correction Workflow

When correcting format issues:

1. **Read the file** to identify issues
2. **Use Edit tool** to fix specific problems
3. **Focus on**:
   - Removing unwanted blank lines
   - Adding required blank lines (before `##` and ending `>`)
   - Removing incorrect `#` headings
   - Ensuring no trailing blank lines

### Example Correction

**Before**:
```markdown
## 标题

内容第一段。

内容第二段。
```

**After Edit**:
```markdown
## 标题
内容第一段。
内容第二段。
```

## Special Cases

### 知识卡片双链格式

When inserting double-links into source documents:
- Format: `[[{WORK_DIR}/项目/.../知识卡片/01. xxx|01. xxx]]`
- Add on new line (not blank line separation)
- Use alias syntax for display

### 输出卡片无末尾引用块

Output cards specifically do NOT need ending `> [[知识卡片]]` because:
- The relationship is tracked in `项目信息.md`
- Output cards are derived from knowledge cards
- Avoids redundancy in the final output

## Additional Resources

For detailed format specifications:
- **`references/knowledge-card-format.md`** - Complete knowledge card format rules
- **`references/output-card-format.md`** - Complete output card format rules

## Quick Reference

**Knowledge Card Checklist**:
- ✅ Blank line only before `##` and final `>`
- ✅ No blank lines anywhere else
- ✅ Ending `> [[source]]` 引用块 present
- ✅ No `#` level 1 heading

**Output Card Checklist**:
- ✅ Absolutely no blank lines anywhere
- ✅ No `#` level 1 heading
- ✅ Content starts with `##`
- ✅ No ending `> [[source]]` 引用块

**When in doubt**: Follow the "tight packing" principle - minimize blank lines to only the two exceptions for knowledge cards, zero for output cards.
