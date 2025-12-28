# Vibe Writing - 参考文档

**v1.0.0 更新**：架构设计文档已内联到 vibe-workflow/SKILL.md

---

## 🏗️ v1.0.0 架构更新

**重大变更**：Subagent 调研+整理模式，主 Agent 指令执行模式

### v1.0.0 架构

```
主 Agent (vibe-workflow)
    │
    ├── 路由决策
    ├── 解析 Subagent 返回的指令
    ├── 执行指令（Bash/Write/Edit）
    └── 强制验证
            │
            ▼
    Subagent (learning/structure/writing/draft)
    │
    ├── 调研（WebSearch）
    ├── 信息整理
    ├── 逻辑分析
    └── 生成标准指令返回
```

### 核心变化

| 方面 | v0.9 | v1.0 |
|------|------|------|
| Subagent | 执行工具调用 | 返回指令描述 |
| 主 Agent | 只做路由 | 解析+执行+验证 |
| 验证 | 可选 | 强制 |

---

## 📚 现有文档

### 示例项目

**完整示例**：`../../项目/Claude和GPT的区别/`

---

## 🔗 相关资源

- **Skills**: `../skills/` - 核心技能（vibe-workflow 已内联架构）
- **Agents**: `../agents/` - 四个工作流Agent（v1.0 调研+整理模式）
- **Commands**: `../commands/` - 5个快捷命令

---

## 📖 使用建议

1. **阅读 vibe-workflow/SKILL.md** - 了解完整架构设计
2. **查看示例项目** - 了解 Vibe Writing 的实际效果
3. **阅读 agent 文件** - 了解各阶段具体工作流程

---

**v1.0.0 更新日期**: 2025-12-28
