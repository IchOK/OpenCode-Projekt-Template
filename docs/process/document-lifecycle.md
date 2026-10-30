---
id: PROC-0001
title: "Document Lifecycle and Multi-Agent Implementation Process"
status: Draft
version: 1.1.0
created_at: 2026-10-29
approved_by: null
approved_at: null
---

# Document Lifecycle and Multi-Agent Implementation Process

Version 1.1.0 supersedes 1.0.0 (approved 2026-10-24) and awaits human re-approval.

## 1. Document types
- Requirements (REQ): human-approved requirements.
- Functional Specifications (FSPEC): human-approved expected behavior and acceptance criteria.
- Architecture Decision Records (ADR): human-approved architecture decisions.
- Tasks (TASK): agent-managed execution units, no separate human approval.
- Implementation Reports (IMP): coordinator-maintained evidence of delivered work, no separate human approval.
- Execution Logs: machine-collected activity evidence, not a governance document.
- Traceability Views: matrices/links derived from document frontmatter; regenerated, not manually maintained, not a governance document (§11).
- Project overview (PROJECT): German context and goals at `project/project.md` (§1.2). Not a substitute for REQ, FSPEC or ADR.

Approved REQ/FSPEC/ADR versions are immutable except through their respective review and human approval process. Tasks and reports are Git-versioned and may evolve within the approved scope.

### 1.1 Governed document review states

REQ, FSPEC and ADR use the same review states:

- `Draft`: editable by the Coordinator; `approved_by` and `approved_at` are
  `null`.
- `In Review`: content is frozen while the named version awaits a human
  decision. The Coordinator may submit a complete draft for review.
- `Approved`: set only after an explicit human approval naming the document ID
  and exact version. Record the approver and approval timestamp.
- `Rejected`: set only after an explicit human rejection naming the document ID
  and exact version. Keep approval fields `null` and record the decision and
  reason in the document's Human review section.

Allowed transitions:

- `Draft` → `In Review`
- `In Review` → `Draft` when review changes are requested
- `In Review` → `Approved` or `Rejected` by explicit human decision only
- `Rejected` → `Draft` for a revised version

There is no direct `Draft` → `Approved` transition. An `Approved` version never
returns to `Draft`. Revising it requires an explicit human instruction naming
the approved ID and version, a version increment and a new `Draft` revision.
The previous approved version remains immutable and recoverable from Git
history. Changes after a rejected review also increment the version before the
document returns to `Draft`.

**Language of artefacts (Coordinator must follow):**
- Project overview — `project/project.md`, template `docs/templates/project.md`, example `project/project-example.md`: German body, headings and table headers. YAML field names and status tokens stay English.
- REQ, FSPEC, ADR, TASK, IMP, execution logs and process documents: English.

**Governed artefact frontmatter:** REQ, FSPEC, ADR and PROC documents use `created_at` for the document creation date (not `date`). Approval timestamps use `approved_at`.

### 1.2 Project overview (PROJECT)

The active project overview (`project/project.md`, id `PROJECT-…`) is a **separate document type**. It does not use the REQ/FSPEC/ADR review skills or the state machine in §1.1. It provides context and roadmap language only; implementation authority always flows from approved FSPEC versions and explicit start instructions (§2–§3).

**Frontmatter semantics:**

- `status` — lifecycle of the overview itself, not of requirements or code:
  - `Draft`: editable; default for new or heavily revised overviews.
  - `Active`: the human accepts this overview as the current project baseline for context (may still change without a formal REQ cycle).
  - `Archived`: retained for history; not the live overview.
- `approved_by` / `approved_at` — optional record that a human accepted a **context snapshot** (who/when). This is **not** approval of REQ, FSPEC or ADR and **not** implementation authorization.
- `review_required` — optional list of pointers to open governance work (for example pending REQ/FSPEC IDs). It does not change `status` and is not an approval queue for the overview file.
- `references` — index of linked REQ/FSPEC/ADR ids for navigation; truth remains in the governed documents.
- `version` / `modified_at` — ordinary document versioning; bump when the overview changes materially.

The Coordinator may update the overview in German when the human directs; OpenCode may prompt before edits (`ask` on `project/**`). Workers do not edit `project/**`.

## 2. Approval is not authorization
Approval of a FSPEC does not permit code changes. Implementation requires a separate, explicit human start instruction naming FSPEC ID and version. Log authorization identity, timestamp and FSPEC version. Never infer authorization from status, milestones, an available task or the completion of another FSPEC.

## 3. Separate implementation states
- `Not Started`: not yet prepared.
- `Ready to Start`: FSPEC approved; prerequisites checked; awaiting explicit human start instruction.
- `In Progress`: explicitly authorized; work underway.
- `Blocked`: affected implementation cannot progress pending resolution.
- `Completed`: all tasks integrated, acceptance criteria evidenced, tests passed, documentation complete.

Coordinator may set `Ready to Start`, but only an explicit human start instruction permits transition to `In Progress`. A `Completed` implementation does not authorize the next FSPEC.

Implementation status lives on the versioned FSPEC task plan (`docs/tasks/FSPEC-<ID>-v<version>.md`), not on the FSPEC approval `status` field.

At most **one** FSPEC implementation may be `In Progress` project-wide at a time (across all versioned task plans under `docs/tasks/`). Starting another FSPEC requires the current one to reach `Completed`, `Blocked` with human resolution, or an explicit human instruction to abandon or pause the active run — not merely completing a single task within it. Worker parallelism ([`multi-agent-orchestration.md`](multi-agent-orchestration.md) §4) applies to **tasks** inside the active FSPEC, not to multiple concurrent FSPEC implementations.

## 4. Coordinator
Normative role: [`multi-agent-orchestration.md`](multi-agent-orchestration.md) §2.1.

Lifecycle duties: reviews approved FSPECs and ADRs; creates and assigns tasks; owns task plans, TASK files, implementation reports and execution logs; may draft REQ, FSPEC and ADR for human approval; manages dependencies and blockers; reviews Worker changes; performs Git integration (merge/rebase allowed, no substantive application-code edits); arranges integration tests. A substantive code conflict must be delegated to a Worker. This document is not an OpenCode ambient instruction; the Coordinator reads it on demand.

## 5. Workers
Normative role: [`multi-agent-orchestration.md`](multi-agent-orchestration.md) §2.1.

Lifecycle duties: implement the assigned task in an isolated Git worktree; run the tests named in the Coordinator prompt; return an English handoff (task ID, branch/commit, files changed, tests, known issues). Workers do not write REQ, FSPEC, ADR, task plans, TASK files or implementation reports, and they do not merge into the integration branch.

## 6. Parallelization
Project-wide maximum is the value configured in [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) (single source of truth); simultaneous implementation Workers, excluding the Coordinator, may not exceed it. This is a ceiling, not a target. OpenCode does not natively cap how many `worker` sessions may run. Coordinator must inspect task dependencies, overlapping files, shared interfaces and predicted merge cost. Run in parallel only if changes are clearly separable and integration is expected to be straightforward; otherwise run sequentially or repartition. Document the reasoning. Subagent sessions do not guarantee worktree isolation; use OpenCode `worktree_create` and `session_move`.

## 7. Task workflow
The concrete workflow chain of the active project is documented in its project description, §9: [`../../project/project.md`](../../project/project.md).

1. Plan from approved, explicitly authorized FSPEC version.
2. Create or confirm the versioned FSPEC integration branch from the configured
   base branch and record both branches in the task plan.
3. Assign file scopes and dependencies.
4. Establish distinct task worktrees from the FSPEC integration branch and
   ensure Worker slots are available.
5. Delegate implementation and tests.
6. Collect results and review against FSPEC/ADRs.
7. Integrate only acceptable work into the FSPEC branch and verify integration
   tests.
8. Mark a task `Completed` only after integration and verification. Remove its
   clean worktree without force when configured; preserve its task branch.
9. After all tasks pass, merge the reviewed FSPEC branch into the configured
   base branch and run final integration tests.
10. Record the implementation report and notify the human at FSPEC completion;
    then wait.

Task states: `Open`, `In Progress`, `Blocked`, `Review Required`, `Completed`, `Cancelled`. `Review Required` signals a decision needed; it does not grant authority to change specs.

## 8. Failure policy
Classify errors as (a) correctable within approved scope, (b) integration conflicts resolvable within approved scope, (c) fundamental or approval-requiring blockers. Every failure needs a cause/hypothesis and evidence. Retry only with a reasoned change, never a blind repetition. Default maximum is the centrally configured number of correction attempts **per issue**. Coordinator may propose increasing the limit for a specific task only with explicit human approval, recorded with rationale and new limit. Stop immediately when an approved spec/ADR would be violated. On limit exhaustion, block affected task and notify human; unrelated authorized tasks may continue. Success requires objective verification (tests/acceptance evidence), not only model judgment.

## 9. Logging and provenance
When [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) `logging.automatic` is `false` (template default), the Coordinator **writes** execution logs manually after task integration using [`../templates/execution-log.yaml`](../templates/execution-log.yaml), populated from the Worker handoff and verified integration results. Capture where available: task ID, agent/role, actual model identifier, task status, integrated commits, test outcomes, failure reasons. Optionally capture session IDs, timestamps, duration, tokens and costs when inexpensive. Missing optional fields stay `null` or are noted unavailable — never invented — and must not block completion. Maintain a detailed execution log separately from the concise implementation report. If a task uses multiple runs/models, preserve each run. A future `logging.automatic: true` requires a proven collector (see template verification CHECK-11), not prompt-only behavior.

## 10. Tool independence
Functional Specifications do not contain model assignments. Model-to-role mapping and provider endpoint belong in runtime configuration (OpenCode config and the host environment). OpenCode is the current platform choice. Any architecture decision must be approved through ADR lifecycle.

## 11. Traceability
Traceability edges (REQ → FSPEC → TASK → evidence) live in the frontmatter of the lower-level document, which is the single source of truth. The templates carry the field layout only; this section defines how the fields are structured and used.

**Field placement:**
- REQ (`docs/templates/requirement.md`): contains no downstream implementation
  links. Reverse relationships are derived from lower-level artefacts so an
  approved REQ version never needs a traceability update.
- FSPEC (`docs/templates/functional-specification.md`):
  `references.requirements` / `references.adrs` name the upstream documents;
  each acceptance criterion has a stable AC ID and names the REQ it realizes.
  It contains no post-approval result or evidence fields.
- TASK (`docs/templates/task.md`): layout only. `id` follows the numbering convention in `docs/templates/tasks-for-fspec.md`. `status` is the task state (§7), not FSPEC implementation status (§3). `references.functional_spec.acceptance_criteria` lists the AC IDs the task covers — this is the primary task-side trace edge. `repository` is `null` when the work is in this repository; otherwise it names the separate code repository. `result_commit` is filled on integration: a commit SHA in this repository, or `repo@sha` when `repository` is set.
- Task plan (`docs/templates/tasks-for-fspec.md`): FSPEC implementation status and start authorization live here (§3). Stored at `docs/tasks/FSPEC-<ID>-v<version>.md` — **one plan file per authorized FSPEC version** (filename encodes the same version as the start instruction and integration branch). The per-task `Acceptance criteria` line mirrors the TASK trace edge for fast plan-level reading. Task IDs are assigned only by this file's numbering convention.
- Implementation report (`docs/templates/implementation.md`): maps every
  acceptance criterion to its tests and evidence and records integrated tasks
  and commits. Stored at `docs/implementation/IMP-FSPEC-<ID>-v<version>.md` for
  the same authorized version. Execution logs retain run-level details.

**Usage rules:**
- Task plan and implementation report paths must include the authorized FSPEC version (`FSPEC-<ID>-v<version>.md`, `IMP-FSPEC-<ID>-v<version>.md`). A new authorized version gets new files; Git history retains completed runs.
- A task without an AC reference is a warning the Coordinator must resolve before review; an AC without any covering TASK is visible in the task plan as a gap.
- An FSPEC is considered fully traced when every acceptance criterion references at least one REQ (or is explicitly marked as project-context-derived) and every AC is covered by a TASK once implementation is `In Progress`.
- At `Ready to Start` and at FSPEC completion, the Coordinator checks trace completeness; gaps are noted in the implementation report, not silently filled.
- Reverse questions must be answerable without guessing: given any commit, task
  or evidence file, the originating AC and REQ must be identifiable through
  TASK and implementation-report references alone. Derived reverse views never
  write back into approved REQ or FSPEC versions.
- Aggregated views (e.g. a REQ ↔ FSPEC ↔ TASK ↔ evidence matrix) are derived artefacts, regenerated when needed, never manually maintained; they inherit the non-governance status from §1.

## 12. Directory Structure and Artefact Placement

The following directory structure is binding for all artefacts of this process:

```text
<repo>/
├── AGENTS.md
├── README.md
├── config/
│   └── agent-policy.yaml
├── .opencode/
│   ├── opencode.jsonc       # OpenCode V2 project config
│   ├── agents/              # coordinator (primary), worker (subagent)
│   ├── skills/              # submit-for-review, record-approval, record-rejection, revise-governed-doc, start-fspec, delegate-task, integrate-task, implementation-report
│   └── commands/            # matching /slash commands for the skills above
├── docs/
│   ├── process/             # Governance documents (PROC-0001, PROC-0002, …)
│   ├── templates/           # Templates for REQ, FSPEC, ADR, TASK, IMP, …
│   ├── requirements/        # Approved requirements (REQ-…)
│   ├── functional_specs/    # Approved functional specifications (FSPEC-…)
│   ├── adr/                 # Architecture decision records (ADR-…)
│   ├── tasks/               # FSPEC task plans and TASK-… files
│   ├── implementation/      # Implementation reports (IMP-FSPEC-…)
│   ├── execution/           # Execution logs (not a governance document)
└── project/
    ├── project.md           # Active project overview (German)
    ├── project-example.md   # Labelled example only; not normative
    └── todo-register.md
```

Optional, not binding: `docs/hardware/`, `docs/external/`, and `vendor/` (Git-ignored reference clones). Create them when the project needs them.

### Placement rules
- Governance documents are stored in `docs/process/`.
- Document templates are stored in `docs/templates/`.
- Approved requirements are stored in `docs/requirements/`.
- Approved functional specifications are stored in `docs/functional_specs/`.
- Architecture decision records are stored in `docs/adr/`.
- FSPEC task plans (`FSPEC-….md`) and individual tasks (`TASK-….md`) are stored in `docs/tasks/`.
- Implementation reports are stored in `docs/implementation/`.
- Execution logs are stored in `docs/execution/`.
- OpenCode agents, skills, commands and project config are stored in `.opencode/`.
- Hardware documentation, when used, is stored in `docs/hardware/`.
- External artefacts (e.g. data sheets), when used, are stored in `docs/external/` and are referenced from the governing requirement, functional specification or ADR.

## 13. Approval and history

Approver: pending (1.1.0 awaits human re-approval)  
Approved version/date: 1.0.0 / 2026-10-24 (superseded)

| Version | Date | Change |
|---|---|---|
| 1.0.0 | 2026-10-24 | Approved by human instruction. |
| 1.1.0 | 2026-10-29 | Directory structure: `.opencode/`, `docs/tasks/`, `docs/execution/`; hardware/external optional; OpenCode worktrees; native concurrency cap documented as absent; project overview language is German; TASK field semantics in §11; process docs are Coordinator on-demand reads; §4–§5 defer role definition to PROC-0002 §2; define governed-document review states and transitions. |
