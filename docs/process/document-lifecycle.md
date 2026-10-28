---
id: PROC-0001
title: "Document Lifecycle and Multi-Agent Implementation Process"
status: Approved
version: 1.0.0
date: 2026-10-20
approved_by: "Projektverantwortlicher (human instruction, 2026-10-24)"
approved_at: 2026-10-24
---

# Document Lifecycle and Multi-Agent Implementation Process

## 1. Document types
- Requirements (REQ): human-approved requirements.
- Functional Specifications (FSPEC): human-approved expected behavior and acceptance criteria.
- Architecture Decision Records (ADR): human-approved architecture decisions.
- Tasks (TASK): agent-managed execution units, no separate human approval.
- Implementation Reports (IMP): coordinator-maintained evidence of delivered work, no separate human approval.
- Execution Logs: machine-collected activity evidence, not a governance document.
- Traceability Views: matrices/links derived from document frontmatter; regenerated, not manually maintained, not a governance document (§11).

Approved REQ/FSPEC/ADR versions are immutable except through their respective review and human approval process. Tasks and reports are Git-versioned and may evolve within the approved scope.

## 2. Approval is not authorization
Approval of a FSPEC does not permit code changes. Implementation requires a separate, explicit human start instruction naming FSPEC ID and version. Log authorization identity, timestamp and FSPEC version. Never infer authorization from status, milestones, an available task or the completion of another FSPEC.

## 3. Separate implementation states
- `Not Started`: not yet prepared.
- `Ready to Start`: FSPEC approved; prerequisites checked; awaiting explicit human start instruction.
- `In Progress`: explicitly authorized; work underway.
- `Blocked`: affected implementation cannot progress pending resolution.
- `Completed`: all tasks integrated, acceptance criteria evidenced, tests passed, documentation complete.

Coordinator may set `Ready to Start`, but only an explicit human start instruction permits transition to `In Progress`. A `Completed` implementation does not authorize the next FSPEC.

## 4. Coordinator
Reviews approved FSPECs and ADRs, creates and assigns tasks, manages dependencies and blockers, reviews changes, performs Git integration operations, arranges integration tests and ensures documentation. The Coordinator **must not** implement or correct production code. A substantive code conflict must be delegated to a Worker. Integration actions such as merge/rebase are allowed, but no substantive application-code edits. Binding agent instruction summary: [`../../AGENTS.md`](../../AGENTS.md).

## 5. Workers
Workers implement, test and document assigned tasks in isolated Git worktrees/branches. They must not alter approved governance documents or merge into the integration branch. Each handoff identifies task, branch/commit, changed files, tests and known issues. Binding agent instruction summary: [`../../AGENTS.md`](../../AGENTS.md).

## 6. Parallelization
Project-wide maximum is the value configured in [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) (single source of truth); simultaneous implementation Workers, excluding the Coordinator, may not exceed it. This is a ceiling, not a target. Coordinator must inspect task dependencies, overlapping files, shared interfaces and predicted merge cost. Run in parallel only if changes are clearly separable and integration is expected to be straightforward; otherwise run sequentially or repartition. Document the reasoning. Subagent sessions do not guarantee worktree isolation. Technical enforcement status is tracked in [`multi-agent-orchestration.md`](multi-agent-orchestration.md) §7.

## 7. Task workflow
The concrete workflow chain of the active project is documented in its project description, §9: [`../../project/project.md`](../../project/project.md).

1. Plan from approved, explicitly authorized FSPEC version.
2. Assign file scopes and dependencies.
3. Establish distinct worktrees and ensure worker slots are available.
4. Delegate implementation and tests.
5. Collect results and review against FSPEC/ADRs.
6. Integrate only acceptable work, verify integration tests.
7. Mark a task `Completed` only after integration and verification.
8. Record implementation report and notify human at FSPEC completion; then wait.

Task states: `Open`, `In Progress`, `Blocked`, `Review Required`, `Completed`, `Cancelled`. `Review Required` signals a decision needed; it does not grant authority to change specs.

## 8. Failure policy
Classify errors as (a) correctable within approved scope, (b) integration conflicts resolvable within approved scope, (c) fundamental or approval-requiring blockers. Every failure needs a cause/hypothesis and evidence. Retry only with a reasoned change, never a blind repetition. Default maximum is the centrally configured number of correction attempts **per issue**. Coordinator may propose increasing the limit for a specific task only with explicit human approval, recorded with rationale and new limit. Stop immediately when an approved spec/ADR would be violated. On limit exhaustion, block affected task and notify human; unrelated authorized tasks may continue. Success requires objective verification (tests/acceptance evidence), not only model judgment.

## 9. Logging and provenance
Automatically capture, where available: task ID, agent/role, actual model identifier, task status, integrated commits, test outcomes, failure reasons. Optionally capture agent calls, session IDs, timestamps, duration, tokens and costs when inexpensive. Missing optional data is noted, never invented, and must not block completion. Maintain a detailed execution log separately from the concise implementation report. If a task uses multiple runs/models, preserve each run.

## 10. Tool independence
Functional Specifications do not contain model assignments. Model-to-role mapping and provider endpoint belong in runtime configuration. OpenCode is the current platform choice; the orchestration mechanism, concurrency enforcement, worktree lifecycle and logging extraction are verified in [`multi-agent-orchestration.md`](multi-agent-orchestration.md) §7. Any architecture decision must be approved through ADR lifecycle.

## 11. Traceability
Traceability edges (REQ → FSPEC → TASK → evidence) live in the frontmatter of the lower-level document, which is the single source of truth. The templates carry the field layout only; this section defines how the fields are structured and used.

**Field placement:**
- REQ (`docs/templates/requirement.md`): `implemented_in.functional_specs` / `implemented_in.adrs` — optional mirror pointing at the documents that realize the requirement. Primary truth stays in the FSPEC; the mirror may lag and is never the basis for decisions.
- FSPEC (`docs/templates/functional-specification.md`): `references.requirements` / `references.adrs` name the upstream documents; each acceptance criterion carries a `traces:` block with `requirement` (REQ it realizes) and `verification` (test/evidence references, filled once integrated).
- TASK (`docs/templates/task.md`): `references.functional_spec.acceptance_criteria` lists the AC IDs the task covers — this is the primary task-side trace edge.
- Task plan (`docs/templates/tasks-for-fspec.md`): the per-task `Acceptance criteria` line mirrors the same edge for fast plan-level reading.

**Usage rules:**
- A task without an AC reference is a warning the Coordinator must resolve before review; an AC without any covering TASK is visible in the task plan as a gap.
- An FSPEC is considered fully traced when every acceptance criterion references at least one REQ (or is explicitly marked as project-context-derived) and every AC is covered by a TASK once implementation is `In Progress`.
- At `Ready to Start` and at FSPEC completion, the Coordinator checks trace completeness; gaps are noted in the implementation report, not silently filled.
- Reverse questions must be answerable without guessing: given any commit, task or evidence file, the originating AC and REQ must be identifiable through references alone.
- Aggregated views (e.g. a REQ ↔ FSPEC ↔ TASK ↔ evidence matrix) are derived artefacts, regenerated when needed, never manually maintained; they inherit the non-governance status from §1.

## 12. Directory Structure and Artefact Placement

The following directory structure is binding for all artefacts of this process:

```text
<repo>/
├── AGENTS.md
├── config/
│   └── agent-policy.yaml
├── docs/
│   ├── process/             # Governance documents (PROC-0001, PROC-0002, …)
│   ├── templates/           # Templates for REQ, FSPEC, ADR, TASK, IMP, …
│   ├── requirements/        # Approved requirements (REQ-…)
│   ├── functional_specs/    # Approved functional specifications (FSPEC-…)
│   ├── adr/                  # Architecture decision records (ADR-…)
│   ├── implementation/      # Implementation reports (IMP-FSPEC-…)
│   ├── poc/
│   │   └── evidence/        # Evidence for orchestration PoC checks (CHECK-…)
│   ├── hardware/            # Hardware documentation
│   └── external/            # External artefacts (data sheets, …)
└── project/
    ├── project.md
    └── todo-register.md
```

### Placement rules
- Governance documents are stored in `docs/process/`.
- Document templates are stored in `docs/templates/`.
- Approved requirements are stored in `docs/requirements/`.
- Approved functional specifications are stored in `docs/functional_specs/`.
- Architecture decision records are stored in `docs/adr/`.
- Implementation reports are stored in `docs/implementation/`.
- PoC evidence is stored in `docs/poc/evidence/`.
- Hardware documentation is stored in `docs/hardware/`.
- External artefacts (e.g. data sheets) are stored in `docs/external/`.

### External artefacts
External files such as data sheets are placed in `docs/external/` and are referenced from the governing requirement, functional specification or ADR.
