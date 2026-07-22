# Project Pulse Implementation Plan

## 1) Goal and scope
Build a small static Project Pulse dashboard that can be previewed from VS Code Run and Debug, using:
- app/index.html
- app/styles.css
- app/project-data.json
- .vscode/launch.json

In scope:
- Render a polished card-based dashboard UI for multiple projects.
- Display project name, owner, status, recentActivity, and priority.
- Ensure deterministic launch behavior that opens index.html from the app directory (not a directory listing).
- Keep implementation suitable for a small exercise repository and CI keyphrase/json checks.

Out of scope:
- Build tooling, frameworks, package managers, or backend services.
- Dynamic APIs or persistence.

## 2) Assumptions/constraints
- Repository is intentionally minimal and currently has an empty app folder.
- Files must satisfy exercise checks that rely on exact phrases and JSON validity.
- Static frontend only; data comes from local JSON file.
- Launch config must be strict JSON (no comments).
- Plan should support Orchestrator-led delegation with clear Designer and Coder boundaries.
- CI checks appear mostly text/structure-based, but manual preview is still required for UX validation.

## 3) File ownership/assignments table

| File | Primary owner | Supporting owner | Responsibility |
|---|---|---|---|
| app/index.html | Coder | Designer | Create semantic dashboard structure, include Project Pulse title, reference styles.css and project-data.json, and render visible project cards using class project-card with status, recentActivity, and priority shown in UI. |
| app/styles.css | Designer | Coder | Define dashboard visual system and responsive layout with polished card styling; include required selectors .dashboard and .project-card plus border-radius and box-shadow for card affordance/readability. |
| app/project-data.json | Coder | Designer (content review) | Provide valid JSON data source with top-level projects array and required fields per project: name, owner, status, recentActivity, priority. |
| .vscode/launch.json | Coder | None | Provide strict JSON launch configuration named Run Project Pulse Dashboard; serve from app directory with python3 -m http.server 5500; configure serverReadyAction to open http://localhost:%s/index.html. |

## 4) Step-by-step implementation plan

1. Confirm requirements and acceptance signals
- Owner: Orchestrator + Planner output consumer.
- Extract mandatory tokens and structure from exercise context:
  - Project Pulse title
  - Required file set
  - Required CSS hooks and data keys
  - Launch naming and URL behavior
- Output: short execution checklist shared with Designer and Coder before edits.

2. Designer defines UI/UX contract first
- Owner: Designer.
- Deliverables:
  - Dashboard information hierarchy (title, project grid, per-card field placement).
  - Styling direction for status/priority readability and responsive behavior.
  - Explicit CSS contract for .dashboard and .project-card.
- Reason for order: prevents Coder from building markup that conflicts with intended visual and accessibility structure.

3. Coder creates data model in app/project-data.json
- Owner: Coder.
- Deliverables:
  - Valid JSON with top-level projects.
  - Multiple representative project objects with all required fields.
- Reason for order: provides concrete schema and sample content used during markup/render decisions.

4. Coder implements structure and rendering in app/index.html
- Owner: Coder, with Designer’s UX contract.
- Deliverables:
  - Semantic structure and Project Pulse title.
  - References to styles.css and project-data.json.
  - Visible project cards using project-card class.
  - UI text paths that include status, recentActivity, priority.
- Dependency: requires Step 2 UX contract and Step 3 data schema.

5. Designer finalizes visual polish in app/styles.css
- Owner: Designer.
- Deliverables:
  - .dashboard layout and spacing.
  - .project-card styling including border-radius and box-shadow.
  - Readable status/priority treatment and responsive adjustments.
- Dependency: can begin after Step 2 and iterate against Step 4 markup.

6. Coder configures run experience in .vscode/launch.json
- Owner: Coder.
- Deliverables:
  - Valid strict JSON.
  - Configuration name exactly Run Project Pulse Dashboard.
  - cwd/app-serving behavior and serverReadyAction URL to index.html.
- Dependency: logically independent of UI content, but best finalized after file paths are confirmed.

7. Integrated validation and cleanup
- Owner: Coder (technical checks), Designer (visual checks), Orchestrator (final decision).
- Validate JSON parsing, keyphrase presence, launch behavior, and first-view UX.
- Resolve any mismatch between content checks and actual browser behavior.

## 5) Dependency map

Technical dependencies:
- app/index.html depends on:
  - app/project-data.json schema/keys (projects, name, owner, status, recentActivity, priority).
  - app/styles.css selectors/classes for expected styling hooks.
- app/styles.css depends on:
  - Finalized HTML class names and section structure.
- .vscode/launch.json depends on:
  - Final folder/file path assumptions (app directory, index.html path).

Task/order dependencies:
- Designer UX contract should precede final HTML/CSS implementation decisions.
- Data schema should exist before final card rendering logic/text mapping.
- Launch config can be drafted early but should be finalized after path conventions are locked.
- Final validation is strictly last, after all four files are in place.

## 6) Parallelization plan

Can run in parallel:
- Designer UX contract draft (layout + accessibility + style intent).
- Coder initial launch.json scaffold.
- Why: no overlapping file edits and low cross-dependency at this early stage.

Partially parallel:
- Designer styles.css work can start while Coder builds index.html, if both align on class contract (.dashboard, .project-card).
- Why: overlap is safe when interface contract is fixed first.

Must run sequentially:
- project-data.json completion before final HTML rendering verification.
- Final launch.json verification after index.html path is finalized.
- End-to-end validation after all files are complete.
- Why: these steps depend on concrete artifacts and exact strings checked by workflow/manual run.

## 7) Validation checklist and pass criteria

| What to verify | How to verify | Pass criteria |
|---|---|---|
| Required files exist | Check filesystem | All four files present: app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json |
| HTML includes required dashboard context | Read app/index.html | Contains Project Pulse, styles.css reference, project-data.json reference, project-card usage, and visible status/recentActivity/priority text paths |
| CSS includes required hooks/polish | Read app/styles.css | Includes .dashboard, .project-card, border-radius, box-shadow, and responsive-friendly layout rules |
| Data schema correctness | Run python3 -m json.tool app/project-data.json | JSON parses; contains top-level projects; each project includes name, owner, status, recentActivity, priority |
| Launch schema correctness | Run python3 -m json.tool .vscode/launch.json | JSON parses with no comments or syntax errors |
| Launch semantics | Inspect .vscode/launch.json | Includes configuration name Run Project Pulse Dashboard and URL opening index.html via serverReadyAction |
| Runtime behavior | Run VS Code launch config manually | Browser opens dashboard page at index.html (not directory listing) and renders project cards with required fields |

Minimum acceptance threshold:
- All checklist rows pass.
- No missing required keys/phrases that would fail workflow checks.
- Manual run confirms first-view dashboard UX is functional and readable.

## 8) Risks and mitigations

Risk: HTML references project-data.json but data is not actually used/rendered in a visible way.
- Mitigation: explicitly include visible project-card markup tied to required fields and verify presence in rendered UI.

Risk: CSS meets keyword checks but looks unpolished or unreadable.
- Mitigation: Designer owns visual pass and accessibility/readability review before final validation.

Risk: launch.json is valid JSON but opens server root or wrong URL.
- Mitigation: enforce app directory serving and explicit http://localhost:%s/index.html serverReadyAction; verify by running launch config.

Risk: Parallel edits cause class/field name drift between HTML, CSS, and JSON.
- Mitigation: establish contract first (.dashboard, .project-card, projects/name/owner/status/recentActivity/priority) and avoid renaming after agreement.

Risk: CI passes but runtime still broken in local preview.
- Mitigation: keep manual run-and-debug check as required gate before handoff.
