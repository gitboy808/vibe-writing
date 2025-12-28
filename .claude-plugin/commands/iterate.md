---
name: iterate
description: 进入迭代对话模式（Vibe Writing 写作Agent）
argument-hint: 无需参数
allowed-tools: ["Read", "Write", "Edit", "Glob", "WebSearch", "Task"]
---

# /iterate - 迭代对话模式

这是 Vibe Writing System 的写作阶段快捷命令 - 迭代模式。

## 执行指令

立即调用以下 Task 工具：

```typescript
Task(
  subagent_type=".claude-plugin:writing-agent",
  prompt="用户希望进入迭代对话模式，对现有输出卡片进行深度对话迭代"
)
```

## 使用场景

当你想要：
- 对现有输出卡片进行深度对话迭代
- 通过对话激发新的思考角度
- 整合对话内容到卡片中

## 执行流程

当你使用 `/iterate` 时，writing-agent 会：

1. **进入对话模式**：不读取文件，直接开始对话
2. **每轮回答**：
   - 深入讲解（基于事实验证）
   - 提炼2-3个延伸问题
   - 引导："继续问 or 说'整理'"
3. **你说"整理"后**：
   - 分析对话解决了什么
   - 设计2-3种认知结构方案
   - 用户选择后整合内容
   - 自动添加卡片到白板
4. **完成后提示**："迭代完成了。调整完说'润色'。"

## 相关命令

- `/polish` - 直接润色语言（跳过对话）
- `/structure` - 深度分析下一步
- `/draft` - 生成最终成稿
