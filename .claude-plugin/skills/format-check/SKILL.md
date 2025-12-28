---
name: format-check
description: Validate and correct file formats for knowledge cards and output cards. Ensures compliance with spacing rules, heading rules, and formatting standards.
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

## 📚 Knowledge Card Organization (内联)

### 🧭 认知轨迹，不是知识逻辑

**核心原则**: 按人类真实的思考过程组织，不是按教科书逻辑。

- ❌ **知识逻辑**: 先讲基础A、B → 再讲主题C（教科书）
- ✅ **认知轨迹**: 尝试理解C → 卡在A → 解决A → 继续 → 卡在B → 解决B → 理解C

**操作方法**:
1. **遇到就打断**: 推导到哪里卡住，就在那里立刻解决，不要延后
2. **用户原始问题**: 小标题直接用用户怎么问的
3. **"求和"重组**: 如果用户说"不理解"→ 重新讲，诊断卡点，按认知轨迹融合内容

**示例**:
```
❌ 错误组织:
## 推导需要的基础
### F=dp/dt
### p=γmv
## 开始推导

✅ 正确组织:
## 开始推导
用到F=dp/dt

## 等等，F不是ma吗？
[解决疑问]

继续推导，用到p=γmv

## 这里又有疑问，p=γmv哪来的？
[解决疑问]

## 完成推导
```

### 📌 卡片标题规则

**核心要求**:
- **具体的问题，不是抽象概念**
- **以用户问题为主，保守润色**
- 中文符号，限50字符

**优化原则**: 不要宏大化、抽象化、学术化

**示例**:
- ❌ 错误: "双生子悖论：为什么对称性会被打破？"（太抽象）
- ✅ 正确: "既然互相看对方时间都慢，那重逢时到底谁更年轻？"（直观）

**理由**: 用户的原始提问往往更直观，不要为了"显得高级"而牺牲直觉性

### ✍️ 写作原则

**极简逻辑，去掉所有修辞**: 钩子是思维方式，不是语言张力

**禁止**:
- ❌ "这听起来很玄乎"
- ❌ "让我们来看看"
- ❌ "这是个有趣的问题"
- ❌ 任何试图用修辞制造张力的表达

**只要**:
- ✅ 纯粹的逻辑关系

**理由**: 真正吸引用户的是**思维逻辑本身**，不是通过修辞、夸张、渲染来制造张力

### 🔢 轮次计数规则

**作用域**: 仅在学习阶段（v0.1，学习Agent）使用。输出阶段不使用轮次计数

**轮次定义**: 1轮 = 用户提问 + AI回答

- 项目启动后AI的引导语**不算**轮次
- 用户看完初始文档后提出第一个问题 → AI回答 = 第1轮
- 以此类推

**轮次持久化**:
1. 回答前：读取项目信息.md → 获取轮次（绝对+相对）
2. 计算：绝对+1，相对+1
3. 标注：【第X轮对话】（用绝对）
4. 判断：相对 == 4 → 触发生成
5. 回答后：立即更新项目信息.md
6. 生成后：重置相对为0

---

## 📦 Output Card Organization (内联)

### 🎯 生成原则（结构Agent专用）

**输出卡片必须比知识卡片更好**:
- ✅ 深度读取知识卡片，提取所有有价值的深刻解释、类比、例子、细节
- ✅ 补充资料（WebSearch），找到更深刻、更清晰的解释
- ✅ 知识卡片 + 补充资料 → 更好的输出卡片
- ❌ 绝不简化有价值的深刻解释

**AI生成前的准备**:
1. **深度读取知识卡片** - 提取所有有价值的深刻解释、类比、例子、细节
2. **识别不足之处** - 使用WebSearch补充资料
3. **融合生成** - 确保输出卡片比知识卡片更好

**生成标准**: 深度、完整、丰富、流畅

**绝对禁止**:
- ❌ 为了简洁而删除深刻解释
- ❌ 为了流畅而牺牲细节
- ❌ 为了精炼而压缩类比例子

### 🧭 认知轨迹，不是知识逻辑

（与知识卡片相同原则，详见上方）

### 📦 节点拆分原则

**核心原则**: 按认知单元拆分，不是按文字段落

**认知单元定义**: 一个完整的思维过程——能让读者产生"哦，原来如此"的最小完整单位

**三个判断标准**:
1. **能独立优化吗？** - 创作者聚焦这段内容时，能专注在"一件事"上
2. **拆开会割裂吗？** - 如果拆成两个节点，中间不会打断一个完整的思考链条
3. **读完有完整收获吗？** - 一个节点读完，应该有一个清晰的"得到了什么"

**结果**: 一张知识卡片 → 通常拆成 3-5 个输出节点

### ✍️ 写作原则

**极简逻辑，去掉所有修辞**（与知识卡片相同原则）

**标准格式**: 输出卡片节点文件**不要一级标题**（`#`），内容直接从二级标题（`##`）开始

### 🎨 润色原则

**润色三原则**:

**原则1：用户框架神圣不可侵犯**
- 用户留下什么 → 保留什么
- 用户删掉什么 → **永不恢复**（即使AI认为重要）
- 用户可能把2000字删到400字 → AI只优化这400字

**原则2：只优化表达，不改变内容**

**禁止做**（绝对）:
- ❌ 调整段落顺序、移动内容位置
- ❌ 恢复用户删掉的内容
- ❌ 添加新观点/新例子/用户没说的内容
- ❌ 改变用户想表达的意思

**可以做**（有限）:
- ✅ 添加简短衔接（1-5字: "可是"、"但是"、"等等"）
- ✅ 补充因果连接词（"这意味着"、"所以"、"因此"）
- ✅ 标题改为问句式，体现认知张力
- ✅ 删除AI腔（"这听起来"、"让我们来看看"、"显然"）
- ✅ 删除明显冗余，但保留情感表达（"太诡异了！"、"凭啥？！"）
- ✅ 优化用户写的不通顺表达（完全保持原意）

**原则3：判断标准**
每处改动都问：**这是让原有内容更清晰，还是添加了新内容？**

**关键检查点**:
- [ ] 段落顺序和小标题位置保持不变？
- [ ] 用户删掉的内容完全没恢复？
- [ ] 没有添加新观点/新例子？
- [ ] 用户写的内容保持原意？
- [ ] 因果关系更清晰？衔接更自然？
- [ ] AI腔删除干净？冗余精简？

**分割线规则**:
- `---` 上面：需要优化的正式内容
- `---` 下面：历史资料/待定（完全不动）

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
