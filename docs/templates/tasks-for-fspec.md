# Tasks for FSPEC-XXXX v1.0.0

Implementation status: Not Started
Start authorization: none

> Keep each task uniquely identified. The coordinator owns this plan. No human task-level approval is required, but an explicit human start authorization is mandatory before code changes.

> Status semantics: Task status (Open, In Progress, …) and FSPEC implementation status (Not Started, Ready to Start, …) are separate dimensions; they are not to be conflated.

Store this plan at `docs/tasks/FSPEC-XXXX.md` and individual tasks at `docs/tasks/TASK-*.md`.

## Numbering convention

> Normative source: Task IDs are assigned only by this convention ([`document-lifecycle.md`](../process/document-lifecycle.md) §11). [`task.md`](task.md) carries field layout only.

- The **prefix marks the kind**: `FSPEC-…` is always a specification, `TASK-…` is always a task. A number such as `1102` may appear only as `FSPEC-…` or `TASK-…`, never as both.
- **Task IDs carry their FSPEC origin**: `TASK-<FF><n>`, where `FF` is the two-digit FSPEC number and `n` is the running index in the plan. FSPEC-0004 → TASK-0401, TASK-0402, …; FSPEC-0009 → TASK-0901, ….
- For FSPEC numbers ≥ 0010 use `TASK-<fspec>-<n>` (for example TASK-0010-1) so the ID cannot collide with two-digit blocks.
- A task plan for a new FSPEC starts at `TASK-<FF>01`.

## Parallelization assessment
Dependencies, file overlaps, interfaces, expected merge effort, selected concurrency (must not exceed `config/agent-policy.yaml` `workers.max_parallel`):

## TASK-0101 — Example
Status: Open  
Assigned to: unassigned  
Depends on: none  
Repository: null  
Files scope: TBD  
Acceptance criteria: TBD  
Result commit: none

Description: TBD

Acceptance evidence: TBD
