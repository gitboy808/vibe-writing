---
name: generate-card
description: 提前生成知识卡片（Vibe Writing 学习Agent）
argument-hint: 无需参数
allowed-tools: ["Read", "Write", "Edit", "Glob", "WebSearch"]
---

# /generate-card - 提前生成知识卡片

这是 Vibe Writing System 的学习阶段快捷命令。

## 使用场景

正常情况下，学习Agent 每4轮对话自动生成知识卡片。

使用 `/generate-card` 可以：
- **提前生成**：不等4轮，立即提炼当前对话
- **手动触发**：在任意对话轮次生成卡片

## 执行流程

当你使用 `/generate-card` 时，learning-agent 会：

1. **先完整回答你当前的问题**
2. **分析对话内容**：
   - 逐轮识别用户的理解目标
   - 判断生成几张卡片
   - 根据理解目标分组
3. **提炼知识卡片**：
   - 开头引用块（疑问来源）
   - 问题式标题
   - 流畅文章形式
   - 结尾引用块（回归主题）
4. **批量处理双链**：
   - 确定插入位置
   - 在源文档中建立连接
5. **更新项目信息**

## 卡片格式

```markdown
> [用户的疑问来源和思考过程]

## [问题式标题]
[内容 - 流畅文章形式]

> [总结这张卡片与项目主题的关系]
[[源文档/知识卡片]]
```

## 相关命令

- `/structure` - 深度分析下一步
- `/iterate` - 迭代优化输出卡片
- `/polish` - 润色语言
