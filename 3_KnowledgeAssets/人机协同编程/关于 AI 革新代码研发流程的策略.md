# 关于 AI 革新代码研发流程的策略

- 架构要 AI 友好。代码组织方式全面拥抱 Monorepo，文档编写要更细致，类型定义完全，示例完备

- 为团队配备 Cursor/Windsurf 等编程 IDE，或提供稳定的 API 服务集成 Cline 等开源工具供同事使用

- AI Code Review 集成到当前 CI 流程

- 接入 Claude Code / Codex 等完全 Agent 能力，打通与 CI 系统的集成，尝试小需求，直接由 Agent 开发，代码提交，功能测试和发布全流程

- 探索内部代码/资料/规范等语料和 AI IDE 的集成

- 接入 Claude CUA，对应用进行功能测试、线上巡检

## test driven vibe coding 最佳实践

- 现在使用copilot，首先生成unit test，然后再生成业务代码。

- 问o3问题也是如此，unit test不通过的error和业务代码一起放context。

