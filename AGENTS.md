# Agent Instructions

## Required project context
Before planning, creating requirements, proposing architecture, delegating tasks, or changing code, read in this order:
1. `./project/project.md` — project purpose, scope, current hardware/software direction and open questions. Template: `docs/templates/project.md`.
2. `docs/process/document-lifecycle.md` — document governance and authorization rules.
3. `docs/process/multi-agent-orchestration.md` — execution rules: roles, worker limits, worktree isolation and verification status of enforcement.
4. Relevant approved Requirements, Functional Specifications and ADRs, including their exact versions.

Treat the project overview as **context, not implementation authorization or an approved specification**. It is currently a draft. If it conflicts with an approved Requirement, FSPEC or ADR, follow the approved artifact and flag the discrepancy. Never convert a project goal or milestone directly into an implementation task without an approved FSPEC and explicit human start instruction.


## External code (vendor/)
- `vendor/` contains external reference repositories (e.g. `vendor/ESP_Framework`) cloned from GitHub. They are not project content, are Git-ignored and must not be modified.
- Reading, searching, indexing or summarizing anything under `vendor/` requires an **explicit human instruction** naming the directory or repository. Without it, agents must not read vendor contents for context, planning or implementation decisions.
- When reading is authorized, treat vendor code strictly as read-only external reference material; it never substitutes for an approved Requirement, FSPEC or ADR.

## Governance
- Normative process documents: [`docs/process/document-lifecycle.md`](docs/process/document-lifecycle.md) and [`docs/process/multi-agent-orchestration.md`](docs/process/multi-agent-orchestration.md).
- Human approval is required for governed Requirements, Functional Specifications and ADRs.
- Approval of a Functional Specification does **not** authorize implementation.
- Implementation requires an explicit human start instruction identifying the FSPEC and version.
- A Coordinator may mark an approved, prepared FSPEC `Ready to Start`, but may not transition it to `In Progress` without that instruction.
- Never automatically start another FSPEC after completion.
- Do not change approved Requirements, FSPECs or ADRs without the applicable human approval process.

## Roles
- Coordinator: plan, assign, review, integrate, arrange integration tests, maintain documentation. Must not implement or correct production application code.
- Worker: implement assigned tasks, test, document outcomes, work only within assigned scope. Must not merge own changes into the integration branch or modify approved governance documents.
- Worker concurrency: at most the maximum configured in `config/agent-policy.yaml` (single source of truth); the coordinator is not counted.
- Each worker must have an isolated Git worktree before editing code. Native OpenCode subagent sessions alone do not establish filesystem isolation.
- Parallelize only tasks with clearly separable changes and low predicted merge effort. Otherwise execute sequentially or repartition.
- Coordinator must document why tasks are parallelized.
- Normative sources for roles, authorization and parallelization: [`docs/process/multi-agent-orchestration.md`](docs/process/multi-agent-orchestration.md) §2–§4 and [`docs/process/document-lifecycle.md`](docs/process/document-lifecycle.md) §4–§5; the rules above are the binding agent-facing summary.

## Tasks and completion
- Tasks are derived from the authorized FSPEC version and may be revised without human approval if scope is unchanged.
- A task is `Completed` only after successful integration and verification.
- FSPEC implementation is complete only when all required tasks are integrated, acceptance criteria proven, tests passed, and implementation documentation complete.

## Failures
- Analyze each failure before retrying. Do not repeat identical attempts without a new hypothesis.
- Default maximum: the correction-attempt maximum configured in `config/agent-policy.yaml` (single source of truth); any increase requires explicit human approval.
- Coordinator may propose a higher limit but needs explicit human consent before changing it for the affected task.
- Block immediately if the correction would violate an approved spec/ADR or requires new approval.
- On exhaustion, mark the affected task `Blocked`, document attempts and notify the human; unrelated authorized work may continue.

## Logging
- Capture task ID, agent/role, actual model identifier, status, commits, tests and failure reasons wherever available automatically.
- Optionally capture calls, timestamps, latency, token use and costs if inexpensive to obtain.
- Do not invent missing telemetry; mark it unavailable. No manual long-form reporting required of workers.
- Keep detailed execution logs separate from the concise implementation report.
