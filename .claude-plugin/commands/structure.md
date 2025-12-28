---
name: structure
description: 深度分析下一步写什么（Vibe Writing 结构Agent）
argument-hint: 无需参数
allowed-tools: ["Read", "Write", "Edit", "Glob", "WebSearch", "Task"]
---

# /structure - 深度分析下一步写什么

这是 Vibe Writing System 的结构阶段快捷命令。

## 执行指令

立即调用以下 Task 工具：

```typescript
Task(
  subagent_type=".claude-plugin:structure-agent",
  prompt="用户希望深度分析下一步写什么，分析知识卡片、识别价值点、生成输出卡片"
)
```

## 使用场景

当你的知识卡片积累到一定程度，想要：
- 分析下一步写什么最合适
- 从全局视角理解选题逻辑
- 获得具体的写作方向建议

## 执行流程

当你使用 `/structure` 时，structure-agent 会：

1. **读取项目全貌**：项目信息、未转化知识卡片、已有输出卡片
2. **核心分析**：
   - 这个选题的核心是什么？
   - 已完成的内容建立了什么基础？
   - 接下来讲什么最合理？
3. **提供2-3个方案**：每个方案包含理由和逻辑
4. **给出AI推荐**：基于认知递进逻辑
5. **等待你的选择**

## 相关命令

- `/iterate` - 迭代优化当前输出卡片
- `/polish` - 直接润色语言
- `/draft` - 生成最终成稿
- `/generate-card` - 提前生成知识卡片
