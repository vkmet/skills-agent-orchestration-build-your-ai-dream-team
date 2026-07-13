# Agent Team

This document describes the custom agent team used to build Mona's **Project Pulse** dashboard. The work is orchestrated using **GitHub Copilot CLI** running inside a GitHub Codespace.

## Agents

### Orchestrator
- **Model:** Claude Opus 4.7 (copilot)
- **Definition:** `.github/agents/orchestrator.agent.md`
- **Responsibility:** Coordinates the full team. Breaks down requests into phases, delegates tasks to Planner, Designer, and Coder, manages parallel vs. sequential execution, enforces file ownership to prevent conflicts, and reports the final outcome.

### Planner
- **Model:** Claude Opus 4.7 (copilot)
- **Definition:** `.github/agents/planner.agent.md`
- **Responsibility:** Researches the codebase and documentation to produce a structured implementation plan with ordered steps, file assignments, dependency mapping, parallel/sequential guidance, edge cases, and validation expectations. Does not write code.

### Designer
- **Model:** Gemini 3.1 Pro (copilot)
- **Definition:** `.github/agents/designer.agent.md`
- **Responsibility:** Handles UI/UX design, accessibility, visual hierarchy, responsive layout, and CSS. Creates the polished dashboard look using project cards, status badges, and deterministic CSS hooks such as `.dashboard` and `.project-card`.

### Coder
- **Model:** GPT-5.5 (copilot)
- **Definition:** `.github/agents/coder.agent.md`
- **Responsibility:** Implements code within the scope assigned by the Orchestrator. Writes HTML/JS/data files for Project Pulse, creates support configuration such as `.vscode/launch.json`, validates the result, and reports what changed and any remaining risk.

## How the team works together

1. The **Orchestrator** receives the high-level request and asks the **Planner** for an implementation plan.
2. The **Planner** researches the repo and returns ordered phases with file ownership and dependency information.
3. The **Orchestrator** runs non-overlapping phases in parallel where possible:
   - The **Designer** produces styling and CSS guidance for the dashboard.
   - The **Coder** implements the HTML structure, data, and launch configuration.
4. For phases that share files or depend on earlier output, the **Orchestrator** sequences them and passes context forward.
5. The **Orchestrator** verifies that the integrated result forms a working Project Pulse dashboard and surfaces any blockers.
6. All git operations (stage, commit, push) are performed manually by the learner through Copilot CLI prompts.
