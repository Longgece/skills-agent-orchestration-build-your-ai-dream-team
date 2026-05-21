# Agent team

Below is the custom agent team used to build Mona's Project Pulse dashboard.

- Orchestrator — Claude Opus 4.7 (copilot): coordinates Planner, Coder, and Designer; delegates tasks and verifies integration. Definition: .github/agents/orchestrator.agent.md
- Planner — Claude Opus 4.7 (copilot): researches the codebase, produces phased implementation plans, lists dependencies and validation steps. Definition: .github/agents/planner.agent.md
- Coder — GPT-5.5 (copilot): implements code, fixes bugs, and prepares runnable app support (e.g., .vscode/launch.json). Definition: .github/agents/coder.agent.md
- Designer — Gemini 3.1 Pro (copilot): handles UI/UX, accessibility, interaction flow, and responsive dashboard visuals. Definition: .github/agents/designer.agent.md

Work will be orchestrated through the GitHub Copilot CLI running in a Codespace to coordinate agents and manage changes.
