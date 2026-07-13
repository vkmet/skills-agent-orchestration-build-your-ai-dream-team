## Summary
Build a static Project Pulse dashboard for Mona using orchestrated specialist roles, with clear file ownership and phase gates that avoid overlap conflicts. The implementation should produce four deliverables: [app/index.html](app/index.html), [app/styles.css](app/styles.css), [app/project-data.json](app/project-data.json), and [.vscode/launch.json](.vscode/launch.json), while satisfying both automated workflow checks and manual run/preview validation.

## Project Pulse Goal
Build Mona's Project Pulse dashboard as a polished, static frontend that reliably renders project health data, supports responsive viewing, and launches predictably in the learner environment.

## Implementation Phases
1. Align scope and ownership before coding
- Owner: Orchestrator
- Files: Planning doc only (this plan in docs/project-pulse-plan.md)
- Outcome: Confirm that Designer and Coder scopes do not overlap in the same phase.

2. Define dashboard data model and seed content
- Owner: Coder
- Files: [app/project-data.json](app/project-data.json)
- Outcome: Valid JSON with top-level projects array; each project includes name, owner, status, recentActivity, and priority.

3. Create launch configuration for deterministic preview
- Owner: Coder
- Files: [.vscode/launch.json](.vscode/launch.json)
- Outcome: Strict JSON, launch name Run Project Pulse Dashboard, serves from app directory, opens index.html via serverReadyAction URL pattern.

4. Design visual system and responsive card styling
- Owner: Designer
- Files: [app/styles.css](app/styles.css)
- Outcome: Polished dashboard styling with .dashboard and .project-card selectors, readable hierarchy, status/priority affordances, border-radius, box-shadow, and responsive behavior.

5. Implement dashboard markup and rendering flow
- Owner: Coder
- Files: [app/index.html](app/index.html)
- Outcome: Title Project Pulse, references styles.css and project-data.json, renders visible project cards using project-card class, and surfaces status, recentActivity, and priority in the UI.

6. Integration pass and consistency check
- Owner: Orchestrator coordinating Designer and Coder
- Files: [app/index.html](app/index.html), [app/styles.css](app/styles.css), [app/project-data.json](app/project-data.json), [.vscode/launch.json](.vscode/launch.json)
- Outcome: No ownership collisions, naming consistency across data and UI, and launch behavior opens dashboard page instead of directory listing.

7. Validation and handoff readiness
- Owner: Coder (execution) + Designer (UX/accessibility review) + Orchestrator (final signoff)
- Files: All four implementation files
- Outcome: Checks pass, preview works, and artifacts are ready for final handoff.

## File Assignments
- Orchestrator
- docs/project-pulse-plan.md (plan governance and phase gating)
- Cross-file integration decisions only; no direct implementation edits unless reassigned.

- Designer
- [app/styles.css](app/styles.css): exclusive ownership in design phase.
- Optional advisory comments for structure expectations (no direct edits to HTML unless explicitly reassigned in a later phase).

- Coder
- [app/project-data.json](app/project-data.json): exclusive ownership.
- [.vscode/launch.json](.vscode/launch.json): exclusive ownership.
- [app/index.html](app/index.html): exclusive ownership after data schema and CSS contract are established.

## Designer Responsibilities
- Define the visual hierarchy for a dashboard-first first impression.
- Ensure .dashboard and .project-card selectors exist and are intentionally styled.
- Provide accessible contrast, spacing, and readable typography scale.
- Encode priority/status affordances so project risk is quickly scannable.
- Ensure responsive layout works cleanly on narrow and wide viewports.
- Report design decisions and any UI risks that require HTML adjustments.

## Coder Responsibilities
- Implement deterministic data contract in project-data.json.
- Implement deterministic launch behavior in launch.json with strict JSON and app-focused serving/open behavior.
- Build index.html structure and rendering logic that consumes project-data.json and displays required fields.
- Preserve agreed CSS hooks from Designer.
- Run syntax/format checks and execute preview validation.
- Report what was validated and what residual risks remain.

## Dependencies Between Steps and Files
- Step 5 depends on Step 2:
- [app/index.html](app/index.html) rendering logic depends on stable field names in [app/project-data.json](app/project-data.json).

- Step 5 depends on Step 4:
- HTML class and layout structure should align with CSS hooks and visual contract in [app/styles.css](app/styles.css).

- Step 7 depends on Steps 2-6:
- Validation requires all four files present and internally consistent, including launch behavior from [.vscode/launch.json](.vscode/launch.json).

- Cross-file contract dependencies:
- Field names in JSON and HTML must match exactly: name, owner, status, recentActivity, priority.
- CSS selectors must match HTML classes exactly: .dashboard and .project-card.

## Parallel Work Decisions
Work that can run in parallel:
1. Step 2 and Step 3 can run in parallel (different files, same owner).
2. Step 4 can run in parallel with Steps 2 and 3 (different file, different owner).

Work that must be sequential:
1. Step 5 must run after Step 2 and Step 4 to avoid schema/class mismatch.
2. Step 6 must run after Step 5 so integration is checked on actual rendered output.
3. Step 7 must run last, after all implementation files are complete.

## Validation Expectations
Automated validation:
- Files exist: [app/index.html](app/index.html), [app/styles.css](app/styles.css), [app/project-data.json](app/project-data.json), [.vscode/launch.json](.vscode/launch.json).
- JSON parses: project-data.json and launch.json.
- Plan-required keyphrases are present in implementation:
- index.html contains Project Pulse, styles.css reference, project-data.json reference, project-card, status, recentActivity, priority.
- styles.css contains .dashboard, .project-card, border-radius, box-shadow.
- launch.json contains Run Project Pulse Dashboard and index.html target language.

Manual validation:
1. Run the VS Code launch configuration Run Project Pulse Dashboard.
2. Confirm browser opens dashboard page (index.html), not a directory listing.
3. Confirm multiple project cards render with owner, status, recentActivity, and priority visible.
4. Check responsive behavior on narrow viewport.
5. Quick accessibility sanity checks: heading hierarchy, readable contrast, keyboard focus visibility (if interactive controls are added).

## Edge Cases to Handle
- Missing or malformed project-data.json: show non-breaking fallback message in UI instead of blank page.
- Empty projects array: render explicit empty state.
- Unexpected status/priority values: render safely with neutral styling fallback.
- Long project names/activity text: wrap without breaking card layout.
- Server not started or wrong cwd in launch config: ensure deterministic app directory serving and index.html target.

## Open Questions / Assumptions
Assumptions:
- The dashboard is static and client-side only; no backend/API integration is required.
- A small seeded projects list is acceptable for initial delivery.
- No external UI framework is required; plain HTML/CSS/JS is acceptable.
- Launch configuration should prioritize predictable learner experience over environment-specific customization.

Open questions:
1. Should status and priority use a fixed enum (for example Active, At Risk, Blocked), or remain free text?
2. Is sorting/filtering required now, or deferred to a future enhancement?
3. Are there branding constraints (colors/fonts/logo) beyond a polished default dashboard style?
