# Agent Team

This document describes the custom agent team used to build Mona's **Project Pulse** dashboard. The work is orchestrated using **GitHub Copilot CLI** running inside a GitHub Codespace.

## Agents

### Orchestrator
- **Model:** Claude Opus 4.7 (copilot)
- **Definition:** `.github/agents/orchestrator.agent.md`
- **Responsibility:** Coordinates the team and delegates work without implementing code directly. It requests planning from Planner first, turns the plan into phases, runs parallel work only when file scopes do not overlap, sequences dependent work, verifies integration quality, and reports outcomes plus blockers.
- **Tools:** `read`, `agent`, `memory`

### Planner
- **Model:** Claude Opus 4.7 (copilot)
- **Definition:** `.github/agents/planner.agent.md`
- **Responsibility:** Researches repository context and relevant docs, then returns an implementation plan only (no code). The plan includes ordered steps, file ownership, dependency mapping, parallel vs. sequential guidance, edge cases, validation expectations, and open questions.
- **Tools:** `read`, `search`, `web`, `memory`, `todo`

### Coder
- **Model:** GPT-5.5 (copilot)
- **Definition:** `.github/agents/coder.agent.md`
- **Responsibility:** Implements code in the file scope assigned by Orchestrator with explicit, testable behavior. For Project Pulse, this includes app code and runnable support setup when assigned (for example `.vscode/launch.json` with strict JSON, `cwd` set to `${workspaceFolder}/app`, and launching `index.html`).
- **Tools:** `read`, `edit`, `search`, `execute`, `web`, `memory`, `todo`

### Designer
- **Model:** Gemini 3.1 Pro (copilot)
- **Definition:** `.github/agents/designer.agent.md`
- **Responsibility:** Owns UI/UX, accessibility, visual hierarchy, responsive behavior, and visual consistency. For Project Pulse, it is expected to produce a polished dashboard with visible project cards, status badges, priority treatment, and deterministic CSS hooks such as `.dashboard` and `.project-card`.
- **Tools:** `read`, `edit`, `search`, `web`, `memory`, `todo`

## How the team works together

1. **Orchestrator** receives the request and delegates research to **Planner**.
2. **Planner** returns a practical plan with file assignments and dependencies.
3. **Orchestrator** converts that plan into phases and assigns explicit file scope to each specialist.
4. **Designer** and **Coder** can run in parallel only when file scopes and dependencies do not conflict.
5. Overlapping or dependent work runs sequentially, with Orchestrator carrying context between phases.
6. Orchestrator verifies integrated output and reports completion status and blockers.

## Team guardrails

- Specialists stay within assigned file scope.
- Planner does not write code.
- Orchestrator coordinates but does not implement code.
- Designer and Coder document decisions, validation, and residual risk in their reports.
- No agent stages, commits, or pushes; git operations remain learner-controlled via Copilot CLI prompts.
