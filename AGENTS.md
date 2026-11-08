# Agent Guidance

Use these shared rules across agents. Follow more specific project or task instructions when they apply, while preserving the user's requested scope.

## Safety &amp; Data Protection

- For tasks involving sensitive data, deletion or overwrite, Git history or branches, or external side effects, follow `.agent-knowledge/static/rules/SAFE-OPERATIONS.md`.

## Understand the Project and Request

- When the user explicitly names a role profile, read its matching file in `.agent-knowledge/static/agents/` and apply its instructions in the current session. If the profile is a placeholder, tell the user it is not yet available.
- Treat the user's request and the repository's current state as the source of truth. Inspect relevant files and instructions before changing anything.
- For code discovery, architecture navigation, and structural questions, prefer knowledge graph tools (`codebase-memory-mcp`) over recursive grep or broad file reads whenever the MCP server is available:
  1. `search_graph` — locate functions, classes, routes, or symbols by pattern.
  2. `trace_path` — trace who calls a function (`inbound`) or what it calls (`outbound`).
  3. `get_code_snippet` — retrieve targeted symbol implementations without reading whole files.
  4. `check_index_coverage` — verify indexed paths and check for unindexed ranges before drawing negative or exhaustive conclusions.
  5. `get_architecture` — inspect high-level system components and module boundaries.
  Fall back to grep, glob, or direct file reading for string literals, error logs, configuration and non-code files (e.g., Dockerfiles, YAML), or when graph coverage is incomplete.
- Identify the project's stack from its manifests and configuration, then read only the applicable guidance under `.agent-knowledge/static/rules/codestyle`.
- Before running project commands or changing dependencies, inspect manifests, lockfiles, runtime-version files, and tool configuration. Follow the declared versions, package manager, and existing scripts. Investigate conflicting declarations; ask before choosing a version or package manager only when the project leaves a necessary choice open.
- Make consequential assumptions and tradeoffs explicit. Ask a focused question when plausible interpretations or an unresolved decision could materially change the result; otherwise proceed using the clearest supported interpretation.

## Plan and Implement Proportionately

- For substantial, multi-step, or architectural work, define the outcome, scope, assumptions, and observable completion criteria, then outline a concise plan. Use the active agent's planning features when available; small, clear tasks can proceed directly.
- When the user explicitly requests multi-agent orchestration, role separation, or parallel work in Orca, follow `.agent-knowledge/static/rules/ORCA-ORCHESTRATION.md`.
- If an important assumption proves false, the approach stops working, or the scope must materially change, pause to explain the change and revise the plan. Get user input when the new scope or decision is not already authorized.
- Prefer the simplest maintainable solution that meets the request. Avoid speculative features, dependencies, abstractions, or rewrites without a concrete need.
- Keep changes surgical: follow the project's architecture and conventions, limit edits to the request and its necessary consequences, remove only artifacts made obsolete by the change, and investigate root causes instead of masking them with avoidable workarounds.
- Before modifying or removing shared functions, methods, types, or endpoints, inspect caller impact and blast radius using `trace_path(direction="inbound")` or `detect_changes` to prevent breaking dependent code.

## Verify and Report

- For substantial work, define how each important outcome will be checked. Choose verification suited to the change: relevant tests, linting, type checks, builds, targeted manual checks, or diff review.
- Run relevant project checks for code changes. For documentation or configuration changes, compare the final diff with the requested scope and validate affected references or formats. Do not claim a check passed unless it was run; report checks that could not be run and what remains unverified.
- Summarize material changes, verification results, and unresolved issues in the final response.

## Task Records and Corrections

- For substantial or long-running work, create the next numbered report under `.agent-knowledge/dynamic/reports/tasks/` using `.agent-knowledge/static/templates/TASKS-TEMPLATE.md`. The template is the sole source for report structure.
- For substantial, multi-stage implementation work, create the next numbered execution plan under `.agent-knowledge/dynamic/workflows/` using `.agent-knowledge/static/templates/WORKFLOW-TEMPLATE.md` before implementation begins. The template is the sole source for workflow structure.
- Record a material architectural decision in `.agent-knowledge/dynamic/docs/notes/ADR-{incremental-number}-{adr-summary}.md` using `.agent-knowledge/static/templates/ADR-TEMPLATE.md` when it sets a durable technical direction or meaningfully changes one. When `codebase-memory-mcp` is available, sync decisions using `manage_adr`.
- For each document type, use the highest existing numeric prefix plus one, starting at 1 when none exists. Check the destination and numbering again before creating the file, especially during parallel work; if the number is already in use, choose the next available number without overwriting existing work.

- Use `.agent-knowledge/dynamic/reports/LESSON.md` for reusable prevention rules, not a chronological task log. Follow `.agent-knowledge/static/templates/LESSON-TEMPLATE.md`; the template is the sole source for lesson structure.
- After a user correction, record a lesson in `.agent-knowledge/dynamic/reports/LESSON.md` when it yields a reusable prevention rule; consult relevant lessons on related work. Do not record task-specific preference changes that do not provide a reusable rule.
- Do not create task-tracking records for trivial work unless the user or project requires them.

## Repository References

- For branch, commit, merge, or pull request work, consult `.agent-knowledge/static/rules/GIT.md` and follow repository conventions and the hosting platform's current state.
- For security requirements during implementation, consult `.agent-knowledge/static/rules/SECURITY-BASELINE.md` only for controls that apply to the feature or deployment.
- For product UI work that affects visual direction, interaction patterns, or the design system, consult the root `DESIGN.md`. Add or update only the affected guidance as implementation and approved decisions evolve; keep current patterns, target decisions, and open proposals distinct.

## Living Project Documents

- Consult `.agent-knowledge/dynamic/docs/README.md` for the document mapping and update rules.
- After a material feature addition, change, or removal, update or retire only the affected information in relevant documents. Treat the listed topics as prompts rather than mandatory headings; keep useful sections concise, leave unknowns explicit, and do not invent decisions.
- Keep external research distinct from project facts. When research is requested or supplied, record its sources and dates and label recommendations or proposed designs; describe the current system using project evidence such as configuration, schemas, migrations, models, and queries. Do not repeat web research during routine implementation unless requested.
