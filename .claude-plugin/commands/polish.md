---
name: polish
description: 直接润色当前输出卡片（Vibe Writing 写作Agent）
argument-hint: 无需参数
allowed-tools: ["Read", "Write", "Edit", "Glob", "WebSearch", "Task"]
---

# /polish - 直接润色语言

这是 Vibe Writing System 的写作阶段快捷命令 - 润色模式。

## 执行指令

立即调用以下 Task 工具：

```typescript
Task(
  subagent_type=".claude-plugin:writing-agent",
  prompt="用户希望直接润色语言，优化表达但不改变内容和结构"
)
```

## 使用场景

当你已经对输出卡片内容满意，想要：
- 优化语言表达
- 润色文字使其更流畅
- 执行事实核查和修正

## 执行流程

当你使用 `/polish` 时，writing-agent 会：

1. **识别最新文件**：读取当前输出卡片
2. **事实核查**（自动）：
   - 识别关键事实点（数字、专有名词、时间、技术规格）
   - WebSearch验证
   - 输出报告 → 用户确认（如需）
3. **处理【】内容**（如有）：
   - 自动分析认知意图
   - 扩写语言
   - 数据核查
   - 直接替换【】
4. **按润色原则优化**：
   - ✅ 用户留下什么就保留什么
   - ✅ **用户删掉什么就永远不恢复**
   - ✅ 用户的结构顺序完全保持
5. **格式检查和保存**

## 相关命令

- `/iterate` - 迭代对话模式（深度优化）
- `/structure` - 深度分析下一步
- `/draft` - 生成最终成稿
