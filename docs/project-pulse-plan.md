Short summary
A practical, phased plan to build the Project Pulse dashboard (lightweight SPA under app/) that shows key project metrics from a JSON file. The plan assumes React + Vite is the preferred stack (with a vanilla‑JS fallback) and provides explicit file assignments (including app/index.html, app/styles.css, app/project-data.json, and a conditional .vscode/launch.json), responsibilities for Designer vs Coder, dependencies, parallelization guidance, validation criteria, estimates, and open questions.

Overview
Goal
- Deliver a small, accessible dashboard ("Project Pulse") that reads project metrics from app/project-data.json and shows them in a clear, mobile‑friendly dashboard UI.
- Provide two implementable pathways (primary: React + Vite; fallback: vanilla JS) while keeping the repo patterns consistent.

Principles / constraints
- Keep project files inside app/ so they are isolated from the existing repo src/ (avoid interfering with existing Vite config) unless the team wants to integrate into main src/.
- Files explicitly assigned in this plan: app/index.html, app/styles.css, app/project-data.json, and .vscode/launch.json (conditional; require permission).
- Designer and Coder roles are explicit per file and phase to avoid overlap.
- Do not commit .vscode/launch.json without permission — keep it optional and documented.

Phases
1) Phase 0 — Kickoff & Decision (setup, decisions, scope)
- Purpose:
  - Confirm chosen stack (React+Vite vs vanilla JS fallback).
  - Confirm scope of metrics, colors, typography, and target breakpoints.
- Files (no code changes yet): docs/style-guide.md (review), docs/agent-team.md (reference).
- Responsibilities:
  - Designer: propose primary layout sketches (desktop + mobile) and color/typography tokens. Provide a 1-page spec (PNG or Figma link).
  - Coder: confirm chosen technical path (React/Vite vs vanilla) and outline dev setup requirements (dev server, build, static hosting).
- Acceptance: agreement on stack and a short visual spec.
- Estimated hours: 2–4.
- Dependencies: none (this is the initial decision).
2) Phase 1 — Data Schema & Mock Data
- Purpose:
  - Define data shape for project metrics and create app/project-data.json (mock data) that covers all UI scenarios and edge cases.
- Files:
  - app/project-data.json — Coder creates and owns the file (designer reviews data labels).
- Responsibilities:
  - Designer: review data keys and provide labels/formatting guidance (dates, percentages, units).
  - Coder: author a robust JSON file containing sample entries including edge cases (missing fields, zeroes, very large numbers, long text).
- Key contents (example fields):
  - projectName, lastUpdated (ISO 8601), health (green/amber/red or numeric %), velocity (number), openIssues, riskNotes, milestones [{id,name,dueDate,status}], contributors [{name,avatarUrl,role}], trends [{date,value}] etc.
- Acceptance:
  - JSON file is valid, covers happy path + at least 4 edge cases, and is documented (top-level comments in plan or example in README).
- Estimated hours: 2–5.
- Dependencies: outcome needed before UI component implementation.
3) Phase 2 — Static HTML/CSS Layout & Visual Design
- Purpose:
  - Build the static layout and styles used by the dashboard (HTML shell & responsive CSS).
- Files:
  - app/index.html — Designer provides static mock + markup suggestions; Coder implements initial shell (semantic HTML).
  - app/styles.css — Designer owns visual style guide (colors, spacing, tokens) and provides final CSS/variables; Coder integrates and may refactor CSS to match components.
- Responsibilities:
  - Designer: supply style tokens, assets (icons or SVGs), and approve the responsive breakpoints and visual hierarchy.
  - Coder: write accessible semantic HTML and baseline CSS (mobile-first) that matches the mock; ensure CSS variables for theme tokens.
- Implementation notes:
  - Keep structure semantic (header, main, sections, nav, footer).
  - Include placeholder components (cards, sparklines placeholders, KPI chips).
- Acceptance:
  - Visuals match the approved mock in desktop and mobile breakpoints; layout graceful degrade when JS disabled.
- Estimated hours: 6–12.
- Dependencies: Phase 1 must be complete (data schema) and Phase 0 decisions locked.
4) Phase 3 — Basic Interactivity & Data Binding (React + Vite primary; vanilla fallback provided)
- Purpose:
  - Hook the static UI to app/project-data.json so that content is rendered from real data.
- Files:
  - app/index.html — will include mounting point or script tag (Coder).
  - app/styles.css — minor updates for dynamic state (Coder).
  - app/project-data.json — used as data source (Coder).
  - Additional: for React path, create app/src/main.jsx, app/src/App.jsx, app/src/components/* (owned by Coder). For vanilla path, create app/main.js and simple module structure.
- Responsibilities:
  - Designer: review component variants and micro-interactions (hover, focus, truncation behaviour).
  - Coder: implement data fetch (local path import or fetch) and mapping to UI. In React path use import of JSON or fetch; in vanilla path use fetch + render functions.
- Implementation choices:
  - React+Vite: scaffold inside app/ with its own package.json (or reuse top-level workspace) — recommend separate package.json inside app/ if isolation is desired.
  - Vanilla: single index.html + app/main.js and small module for rendering.
- Acceptance:
  - All widgets show data from project-data.json; no errors in console for normal cases; basic responsiveness preserved.
- Estimated hours:
  - React path: 8–18 (scaffold + components)
  - Vanilla path (fallback): 6–12
- Dependencies: Phase 2 complete.
5) Phase 4 — Visual polish, accessibility, and keyboard interactions
- Purpose:
  - Enhance UI polish (transitions, tooltips), ensure accessibility, and add keyboard navigation.
- Files:
  - app/index.html — add ARIA landmarks and semantic roles (Coder).
  - app/styles.css — focus styles, high-contrast adjustments (Designer + Coder).
  - For React: components updated to include ARIA attributes and tests.
- Responsibilities:
  - Designer: provide focus/hover style guidance, accessible color checks, and alt text/ARIA suggestions.
  - Coder: implement ARIA, keyboard navigation, skip-to-content link, ensure contrast ratios meet WCAG AA (or document if AAA).
- Acceptance:
  - Keyboard-only navigation works for all interactive elements.
  - Contrast ratio >= 4.5:1 for normal text; 3:1 for large text (WCAG AA).
  - Run Lighthouse accessibility audit score >= 90 targeted (or documented exceptions).
- Estimated hours: 4–10.
- Dependencies: Phase 3 complete.
6) Phase 5 — Error states, edge cases, and offline behaviour
- Purpose:
  - Add graceful fallback when project-data.json missing / malformed, and add “no data” or “partial data” UI states. Optionally implement local caching (localStorage) for offline read.
- Files:
  - app/index.html, app/styles.css — add markup/classes for empty/error states (Coder).
  - app/project-data.json — ensure example malformed scenarios exist for testing (Coder).
- Responsibilities:
  - Designer: produce designs for empty state, error state, and loading skeletons.
  - Coder: implement the states and tests to simulate them.
- Acceptance:
  - Dashboard shows distinct UIs for: loading, empty, partial, stale data (older than N days), and error (parse/fetch error).
- Estimated hours: 4–8.
- Dependencies: Phase 3 & 4 complete.
7) Phase 6 — Tests, documentation, and CI readiness
- Purpose:
  - Add basic automated tests (unit or smoke), document run instructions, and prepare optional .vscode/launch.json.
- Files:
  - docs/project-pulse-plan.md — add plan summary and run instructions (Planner will create this in repo docs).
  - app/ (tests) — e.g., app/tests/* (Coder).
  - .vscode/launch.json — conditional; Coder drafts but does not commit without permission (see below).
- Responsibilities:
  - Designer: review the README/docs for visual acceptance steps.
  - Coder: write simple unit tests (component-level or DOM smoke tests) and a test script in package.json. Add run instructions in docs.
- Acceptance:
  - Tests pass locally (CI-ready). README includes dev/run/build commands and notes about optional .vscode config.
- Estimated hours: 4–10.
- Dependencies: previous phases complete.
8) Phase 7 — Release, handoff, and optional integration
- Purpose:
  - Final checks, build for production, and optionally integrate app into repository root if requested.
- Files:
  - app/* (build artifacts), docs/* for final docs.
- Responsibilities:
  - Designer: sign-off visual QA.
  - Coder: run npm run build (or provide static bundle), recommend hosting instructions.
- Acceptance:
  - Production build works and can be served as static site (dist/). Hand-off includes list of remaining tasks and maintenance notes.
- Estimated hours: 2–6.
- Dependencies: All prior phases complete.

File assignments (explicit)
- app/index.html (required)
  - Purpose: shell + mounting point for SPA or static script tags.
  - Created by: Coder (implements semantic HTML).
  - Reviewed/approved by: Designer (visual and accessibility).
  - Notes: For React path include minimal script that mounts the app; for vanilla include main.js script tag.
- app/styles.css (required)
  - Purpose: all visual styling, CSS variables for tokens, responsive rules, focus styles.
  - Created by: Designer (style tokens & high-level stylesheet) + Coder (integration & responsive tweaks).
  - Owned by: Coder after design sign-off.
- app/project-data.json (required)
  - Purpose: canonical mock and test data for the dashboard (use as a local file import or fetch).
  - Created and maintained by: Coder (with Designer input on labels and formatting).
  - Content: includes happy path and edge cases; documented top-of-file in docs or README.
- .vscode/launch.json (conditional — DO NOT commit without explicit permission)
  - Purpose: convenience for local debugging (launch browser with Vite or open index.html).
  - Created by: Coder drafts it for the team, but mark as conditional. Request permission before committing to repo.
  - Rationale: personal VSCode settings can cause noise in the repo. Keep optional.
- app/src/* or app/main.js (depending on chosen stack)
  - Purpose: component code (React) or rendering modules (vanilla).
  - Created by: Coder.
- docs/project-pulse-plan.md (deliverable of this planning task)
  - Purpose: this plan plus run and validation steps.
  - Created by: Planner (present document).
- docs/README or docs/run-instructions.md
  - Purpose: quick run/build instructions.
  - Created by: Coder (with Designer sign-off).

Dependencies between phases and tasks
- Phase 0 must complete before Phase 1 and 2 (stack + visual direction).
- Phase 1 (data schema) is required before Phase 2 (so the layout can account for data needs) and Phase 3.
- Phase 2 (HTML/CSS shell) must be present before data binding in Phase 3.
- Phase 3 is prerequisite for Phase 4 and Phase 5.
- Phase 4 (accessibility polish) must finish before final tests (Phase 6) so tests include a11y checks.
- Phase 6 tests must pass before Phase 7 release.
- .vscode/launch.json is independent; it can be drafted after Phase 3 but must not be committed without permission.

Parallel work decisions (what can run concurrently and rationale)
- Parallel tasks recommended:
  - Designer: Create high-fidelity mockups, color tokens, and assets can be done in parallel with Coder setting up the repo skeleton and creating project-data.json (Phases 1 and 2 overlap). Rationale: design assets and data schema are complementary and largely independent.
  - Coder: While Designer polishes CSS tokens, Coder can scaffold React app or prepare vanilla script structure and CI test skeleton.
  - Phase 5 (error states) design for empty/error screens can proceed in parallel with Phase 4 (accessibility) because their implementations touch different components and the Designer can provide assets while Coder implements accessibility hooks.
- Work that must be sequential:
  - Implementing interactive components that consume data (Phase 3) must wait for the HTML/CSS base (Phase 2).
  - Final testing (Phase 6) must run after Phase 4 accessibility and Phase 5 error states are implemented.
- Rationale: separating visual and structural work reduces blocking. Data schema is required earlier to avoid rework but can be created while visual mocks are refined.

Validation & Acceptance Criteria
Manual checks (must pass)
- Build & Run
  - React/Vite path:
    - Steps: cd app (if isolated), npm install, npm run dev -> Visit http://localhost:5173 (or configured Vite port). npm run build produces dist/.
    - Acceptance: App loads, no console errors for happy path, data is displayed.
  - Vanilla path:
    - Steps: serve app/ with a static server (python -m http.server 8000) or open index.html in a browser that allows fetch of local files (or use a small dev server). Acceptance: data renders, no console errors.
- Visual QA
  - Desktop and mobile breakpoints visually match approved mock (Designer sign-off).
  - Typography and spacing consistent with tokens.
- Accessibility
  - Keyboard navigation: all interactive items reachable and usable via Tab/Enter/Space.
  - Focus: visible focus indicator for interactive elements.
  - ARIA: landmarks present, labels on non-text controls.
  - Color contrast: pass WCAG AA for body text (4.5:1).
  - Tools: run Chrome Lighthouse or axe/Accessibility Insights — target accessibility score >= 90 with documented exceptions.
- Functional tests
  - Data binding: changing values in app/project-data.json updates UI after page refresh (or hot reload).
  - Edge states: simulate missing fields or malformed JSON and verify corresponding error UI.
  - Offline: if caching implemented, read cached data when fetch fails.

Acceptance checklist (high level)
- All required files exist in app/ and match the agreed design/spec.
- Data-driven UI renders from app/project-data.json with no console errors in normal path.
- Responsive behavior verified at three breakpoints: mobile (≤ 480px), tablet (~768px), desktop (≥ 1024px).
- Accessibility criteria listed above met or documented exceptions accepted by Designer and Product.
- Tests and run instructions documented in docs.

Edge cases, error states & risks to handle
- Missing project-data.json (network or path error).
- Malformed JSON (parsing error).
- Partial data (missing fields like contributors or milestones).
- Very long strings (project names, notes) — test truncation/tooltip strategy.
- Large numbers or unexpected units — apply formatting and fallbacks.
- Slow or absent network (if fetching externally) — show cached or offline state.
- Visual regressions due to global styles in the parent repo — isolate styles in app/ namespaced classes or use scoped styles for React components.
- Conflicting toolchains: repository already contains a Vite-based project in root — avoid naming collisions if adding another Vite project in app/. Decide to integrate or isolate.
- Committing .vscode settings may be controversial — mark as conditional.

Estimates & Timeline (rough)
- Phase 0: Kickoff & Decision — 2–4 hours (1 day)
- Phase 1: Data Schema & Mock Data — 2–5 hours (1 day)
- Phase 2: Static HTML/CSS Layout — 6–12 hours (1–3 days)
- Phase 3: Data Binding & Interactivity — 6–18 hours (2–4 days) [React path upper end]
- Phase 4: Accessibility & Polish — 4–10 hours (1–2 days)
- Phase 5: Error states & offline — 4–8 hours (1–2 days)
- Phase 6: Tests & Documentation — 4–10 hours (1–2 days)
- Phase 7: Release & Handoff — 2–6 hours (1 day)
Total estimated: 30–75 hours (approx 1–3 calendar weeks)

Suggested timeline (example for a 2-week sprint)
- Day 1: Phase 0 + Phase 1
- Days 2–4: Phase 2 (Designer/Coder parallel work)
- Days 5–8: Phase 3 implementation + basic polish
- Days 9–10: Phase 4 accessibility & Phase 5 error states
- Days 11–12: Phase 6 tests and documentation
- Day 13: Final QA/Phase 7 handoff

Work that can run in parallel (specific)
- Designer: finalizes assets, tokens, and mobile/desktop mocks (Phase 2) while
- Coder: creates project-data.json and scaffolds project (Phase 1 + 3 skeleton).
- During Phase 3: Coder implements core data-driven card components while another developer (or same Coder) writes tests and CI skeleton (Phase 6).
Rationale: these tasks are non-blocking and reduce idle time.

Work that must run sequentially (specific)
- Data-driven rendering cannot be fully implemented before the data shape is agreed (Phase 1 → Phase 3).
- Accessibility acceptance testing (Phase 4) must follow implementation of dynamic features (Phase 3).
Rationale: fixes after implementation are more costly.

Validation expectations and test steps (concise)
- Developer local run:
  - React path: cd app && npm ci && npm run dev; open local URL
  - Vanilla: cd app && serve static files (python -m http.server or similar)
- Smoke test:
  - Confirm top KPIs populate from app/project-data.json.
  - Simulate missing keys and verify displayed “No data” UI.
  - Resize viewport to mobile and check layout.
  - Tab through controls to ensure focus order and keyboard responses.
- Automated:
  - Run npm test (unit/smoke).
  - Run a11y check: axe-core or Lighthouse.
- Acceptance: all manual checks pass and CI pipeline passes or documented exceptions accepted.

Open questions & Blockers (require decision)
1. Stack decision (required): Confirm primary stack:
   - Option A (recommended): React + Vite inside app/ (isolated package.json or reuse repo workspace)
   - Option B: Vanilla JS (single-file approach)
   - Note: If React+Vite chosen, do you want the app integrated into root Vite config (src/) or isolated under app/ with its own package.json? Integration may need repository-level changes.
2. .vscode/launch.json: Do we have permission to commit workspace-specific VSCode configs? Plan assumes NO commit without explicit permission.
3. Hosting / integration: Will the dashboard be hosted as a standalone static site or must it be integrated into an existing site/build in the repo? This affects build and CI decisions.
4. Data source: Is app/project-data.json a final source or will the real data come from an API endpoint? If API, provide schema and CORS details.
5. Accessibility target: Is WCAG AA sufficient, or target AAA? Are any accessibility tradeoffs acceptable?
6. CI requirements: Is there an existing CI system to add tests/builds to, or do we provide a recommended GitHub Actions workflow?
7. Ownership & code reviews: Who will review Designer/Coder pull requests for final approval?
8. Visual assets: Are there brand assets (SVGs, logos, fonts) that must be used, or should the Designer use placeholder assets for now?

Immediate recommended tasks (short list)
- Confirm stack choice (React+Vite or vanilla).
- Designer: supply initial mockups (desktop and mobile), color tokens, and a sample KPI list.
- Coder: create an initial app/project-data.json mock file and a minimal app/index.html and app/styles.css skeleton for the Designer to iterate against (no commit performed by Planner; this is guidance).
- Ask permission about committing .vscode/launch.json.

Notes on repo context and integration
- The repository already contains a Vite configuration and root-level src/ (repo-level). Decide whether to leverage the existing Vite setup or isolate the dashboard in app/ (recommended for low-risk, easier removal).
- If integrating into existing root build, adjust file assignments accordingly (we can map app/index.html → src/project-pulse/index.html during integration); discuss with the team.

Deliverables to produce when implementing
- Files to be created in the repo (per this plan):
  - app/index.html
  - app/styles.css
  - app/project-data.json
  - app/src/* (React components) OR app/main.js (vanilla)
  - docs/project-pulse-plan.md (this plan)
  - docs/run-instructions.md
  - optional .vscode/launch.json (conditional)

PLAN_SUMMARY_JSON
{
  "phases": [
    {"id": 0, "name": "Kickoff & Decision", "estimated_hours": "2-4"},
    {"id": 1, "name": "Data Schema & Mock Data", "estimated_hours": "2-5"},
    {"id": 2, "name": "Static HTML/CSS Layout & Visual Design", "estimated_hours": "6-12"},
    {"id": 3, "name": "Basic Interactivity & Data Binding", "estimated_hours": "6-18"},
    {"id": 4, "name": "Accessibility & Polish", "estimated_hours": "4-10"},
    {"id": 5, "name": "Error States & Offline Behaviour", "estimated_hours": "4-8"},
    {"id": 6, "name": "Tests, Documentation & CI Readiness", "estimated_hours": "4-10"},
    {"id": 7, "name": "Release & Handoff", "estimated_hours": "2-6"}
  ],
  "files": [
    "app/index.html",
    "app/styles.css",
    "app/project-data.json",
    ".vscode/launch.json (conditional)",
    "app/src/* (if React)",
    "app/main.js (if vanilla)",
    "docs/project-pulse-plan.md",
    "docs/run-instructions.md"
  ],
  "immediate_tasks": [
    "Confirm stack: React+Vite or vanilla JS",
    "Designer: provide mocks and tokens",
    "Coder: author project-data.json mock file",
    "Decide on .vscode/launch.json commit policy"
  ],
  "blockers": [
    "Stack decision (React + Vite vs vanilla JS)",
    "Permission to commit .vscode/launch.json",
    "Decision about standalone hosting vs integration into existing repo build",
    "If real API is used, need schema and CORS/auth details"
  ]
}
