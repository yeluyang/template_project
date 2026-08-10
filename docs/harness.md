# Harness

项目的 Harness 体系，以 Trellis(<https://docs.trytrellis.app>) 为基本框架，由 Trellis 管理以下方面：

- 工作流：工作流编排，任务管理
- 上下文：长期知识，工作区记忆

Harness 的工作环境 (Env) 方面，当前以开发者本地环境 (或本地 Container) 为主，远程环境 (如 E2B, OpenHands) 暂不考虑。

Harness 的反馈 (Feedback) 方面，包含以下方面：

- Makefile: 统一管理界面
- Test: make test, 单测
- Lint: make lint
- Format: make fmt
- Security: make security, 安全检查 (如：Semgrep, Gitleaks, CodeQL, Trivy 等), 暂不考虑
- CI: make ci, 会在 CI 环境执行以上几乎所有检查

Harness 的观察 (Observability) 与评估 (Evaluation) 方面，会使用以下工具，但是当前暂不考虑实现：

- OpenTelemetry: 监控打点，实时上报 Harness 运行状态
- Langfuse: 记录 Agent 运行 Trace, 评估 Harness 效能

自动化协作 (Automation & Collaboration) 方面，让 Agent 无缝集成到各种工作中，并自动化开始工作：

- Codebase issues: TODO
- Lark Bot: Botmux
- TODO

除以上框架/工具外，还有以下辅助建设补充 Harness 体系：

- 上下文：AGENTS.md, CLAUDE.md. 定义 Repo 上下文入口
- Skills: .agents/skills, .claude/skills. 定义 Repo 共享 Skills
- Roles: .agents/agents, .claude/agents. 定义 Repo 特色 Role 的 Sub-Agent
- Connectivity: 各类 CLI/MCP/Skills 链接 Repo 外的世界
