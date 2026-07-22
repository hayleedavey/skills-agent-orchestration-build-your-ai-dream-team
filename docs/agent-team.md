# Agent team

This project uses a four-agent team in GitHub Copilot CLI (running in a Codespace) to orchestrate and deliver Mona's Project Pulse dashboard.

## Team composition

| Agent | Target model | Responsibility | Agent definition path |
| --- | --- | --- | --- |
| Orchestrator | Opus 4.7 | Coordinates the workflow, delegates tasks, and combines outputs from specialist agents into a cohesive delivery. | `.github/agents/orchestrator.agent.md` |
| Planner | Gemini 3.1 Pro | Breaks requirements into implementable steps, identifies dependencies, and creates execution plans. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 | Implements code changes, runs validations, and fixes technical issues surfaced during execution. | `.github/agents/coder.agent.md` |
| Designer | Opus 4.7 | Defines UX/UI direction, interaction flows, and visual polish for dashboard experiences. | `.github/agents/designer.agent.md` |

## Orchestration note

All agent work is coordinated through GitHub Copilot CLI inside this Codespace environment.
