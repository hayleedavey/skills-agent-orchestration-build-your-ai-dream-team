# Agent team

This project uses a four-agent team in GitHub Copilot CLI (running in a Codespace) to orchestrate and deliver Mona's Project Pulse dashboard.

## Team composition

| Agent | Target model | Responsibility | Agent definition path |
| --- | --- | --- | --- |
| Orchestrator | GPT-5.3-Codex | Coordinates the workflow, delegates tasks, and combines outputs from specialist agents into a cohesive delivery. | `app/agents/orchestrator.md` (planned) |
| Planner | GPT-5.3-Codex | Breaks requirements into implementable steps, identifies dependencies, and creates execution plans. | `app/agents/planner.md` (planned) |
| Coder | GPT-5.3-Codex | Implements code changes, runs validations, and fixes technical issues surfaced during execution. | `app/agents/coder.md` (planned) |
| Designer | GPT-5.3-Codex | Defines UX/UI direction, interaction flows, and visual polish for dashboard experiences. | `app/agents/designer.md` (planned) |

## Orchestration note

All agent work is coordinated through GitHub Copilot CLI inside this Codespace environment.
