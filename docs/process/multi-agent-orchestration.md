---
id: PROC-0002
title: "Multi-Agent Orchestration"
status: Draft
version: 1.1.0
created_at: 2026-10-29
approved_by: null
approved_at: null
---

# Multi-Agent Orchestration

Version 1.1.0 supersedes 1.0.0 (approved 2026-10-24) and awaits human re-approval.

## 1. Scope and applicability

- Applies to every project started on this template, not only the originating one.
- Execution model: one Coordinator (OpenCode Primary Agent) and up to the
  configured maximum number of Workers (native Subagents) for implementation of
  human-approved FSPECs.
- A dedicated orchestration service requires an approved ADR.
- The runtime model is a runtime choice and never part of this rule set (§7).
  Do not pin a model ID in agent files or FSPECs.

## 2. Roles and authorization

This section is the normative role definition. [`document-lifecycle.md`](document-lifecycle.md) §4–§5 restates only lifecycle duties and must not contradict it.

### 2.1 Roles

- **Human**: approves REQ, FSPEC and ADR; issues the implementation start
  instruction; is the only party that may raise the retry limit.
- **Coordinator** (OpenCode primary agent `coordinator`): plans from an
  authorized FSPEC, assigns tasks, reviews Worker output, integrates, and
  arranges integration tests. Owns the document pipeline: task plans, TASK
  files, implementation reports, execution logs; may draft REQ, FSPEC and ADR
  for human approval; writes and updates the German project overview. Speaks
  with the human in German; writes all other artefacts in English. Never
  implements or corrects production application code (merge/rebase of Worker
  work is allowed; no substantive application-code edits). Puts task identity,
  FSPEC version, file scope, acceptance criteria, named ADRs, required tests
  and worktree path in the Worker prompt — the Worker does not read process
  documents. Launches no subagent except `worker`.
- **Worker** (OpenCode subagent `worker`): implements one assigned task in an
  isolated Git worktree and runs the tests named in the prompt. Returns an
  English handoff (task ID, branch/commit, files changed, tests/results,
  failures, blockers, model if available). May change in-scope application
  files and their in-tree comments. Does not write or edit REQ, FSPEC, ADR,
  task plans, TASK files, implementation reports, process documents, `AGENTS.md`,
  `config/` or `.opencode/` (including drafts). Does not merge, rebase, switch
  to the integration branch, or push. Does not load Coordinator skills.

### 2.2 Authorization

Implementation starts only on an explicit human start instruction naming the
FSPEC ID and version. Approval, `Ready to Start`, an available task or the
completion of another FSPEC never substitute for it
([`document-lifecycle.md`](document-lifecycle.md) §2–§3). A completed FSPEC
does not authorize the next one. The concrete workflow chain of the active
project is documented in its project description, §9:
[`../../project/project.md`](../../project/project.md).

### 2.3 Instruction sources

- Shared (every session): [`../../AGENTS.md`](../../AGENTS.md).
- Coordinator: [`.opencode/agents/coordinator.md`](../../.opencode/agents/coordinator.md)
  and Coordinator skills. Process documents and `config/agent-policy.yaml` are
  not OpenCode ambient `instructions`; the Coordinator reads them on demand.
  Project-mode `edit` permissions allow REQ/FSPEC/ADR drafts, tasks,
  implementation reports, and execution logs; they deny `docs/process/**`,
  `docs/poc/**`, `docs/templates/**`, `config/**`, and `.opencode/**`. Edits to
  `project/**` and `AGENTS.md` use `ask`. Template maintenance stays with the
  human (or the IDE), not silent Coordinator changes during implementation.
- Worker: [`.opencode/agents/worker.md`](../../.opencode/agents/worker.md).
  Worker `skill` permissions deny Coordinator skills. Specs for a run come
  from the Coordinator prompt, not from process documents.

## 3. Technical constraints

Normative rules below; the concrete numbers are maintained centrally in
[`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) as the single
source of truth.

- Maximum concurrent implementation Workers, as maintained centrally in
  [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) (single source
  of truth); the Coordinator is not counted. This is a ceiling, not a target.
  OpenCode has no native counter for concurrent sessions of the same subagent.
- Each Worker requires its own isolated Git worktree/branch before any edit;
  native Subagent sessions alone do not establish filesystem isolation. Use
  OpenCode `worktree_create` and `session_move` (configured parent directory:
  `.worktrees`).
- `config/agent-policy.yaml` defines the integration base branch. Each
  authorized FSPEC version uses its own integration branch named
  `<fspec_branch_prefix><FSPEC-ID>-v<version>`. Task worktrees branch from that
  FSPEC branch; reviewed task commits integrate back into it. A completed and
  verified FSPEC branch integrates into the configured base branch.
- After successful task integration and verification, remove the clean Worker
  worktree without force when configured. Preserve task branches for
  traceability. Preserve blocked, failed, unintegrated or dirty worktrees and
  report their paths. Use `git worktree remove` via the Coordinator's allowed
  shell rules, not OpenCode `worktree_remove`: the native tool requires `edit`
  on the worktree directory, which would also permit patching application files
  there and contradicts the Coordinator's no-implementation role.
- Default maximum of correction attempts per issue, as maintained centrally in
  [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml); any increase
  needs explicit human approval, recorded with rationale and new limit.
  OpenCode has no native retry counter.
- Integration requires passing tests, documentation and spec compliance.
- Shell can bypass `edit` permission rules. Coordinator shell is therefore
  `ask` by default; do not treat edit allowlists as a guarantee.

## 4. Parallelization

- Only one FSPEC implementation may be `In Progress` at a time
  ([`document-lifecycle.md`](document-lifecycle.md) §3). Do not authorize or
  record `In Progress` on a second versioned task plan while another FSPEC plan
  is still `In Progress`. `workers.max_parallel` limits concurrent **Workers**
  on tasks for the **active** FSPEC, not multiple FSPEC implementations at once.
- Run tasks in parallel only when file scopes, dependencies and interfaces are
  clearly separable and integration is expected to be straightforward.
- Otherwise run sequentially or repartition the tasks.
- The Coordinator documents the parallelization reasoning in the task plan
  (see [`docs/templates/tasks-for-fspec.md`](../templates/tasks-for-fspec.md)).
- Overlapping-file tasks must be scheduled sequentially.

## 5. Task execution and failure policy

Full rules in [`document-lifecycle.md`](document-lifecycle.md) §7–§8; execution-relevant core:

- Analyze every failure before retrying; retry only with a reasoned change,
  never blind repetition.
- Stop immediately when a correction would violate an approved spec/ADR or
  would require new approval; block the task and notify the human.
- On attempt exhaustion: affected task `Blocked`, attempts documented,
  unrelated authorized work may continue.
- Success requires objective verification (tests/acceptance evidence), not
  only model judgment.

## 6. Logging and provenance

- When `logging.automatic` is `false` in [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml),
  the Coordinator captures on integration (manual execution log). Where available:
  task ID, agent/role, actual model identifier, task status, integrated commits,
  test outcomes, failure reasons.
- Optionally capture calls, session IDs, timestamps, duration, tokens and costs
  when inexpensive. Missing data is marked unavailable, never invented, and must
  not block completion.
- Keep the detailed execution log separate from the concise implementation
  report (templates:
  [`execution-log.yaml`](../templates/execution-log.yaml),
  [`implementation.md`](../templates/implementation.md)).
- Store filled execution logs in `docs/execution/` and reports in
  `docs/implementation/`.
- If a task uses multiple runs or models, preserve each run.

## 7. Tool independence

- Task, implementation report and execution-log formats stay tool-independent.
- FSPECs never contain model assignments; model-to-role mapping and provider
  endpoints belong in runtime configuration.
- OpenCode is the current platform choice, not a permanent commitment; changes
  follow the ADR lifecycle.

## 8. Approval and history

Approver: pending (1.1.0 awaits human re-approval)  
Approved version/date: 1.0.0 / 2026-10-24 (superseded)

| Version | Date | Change |
|---|---|---|
| 0.1.0 | 2026-10-24 | Initial draft: consolidates the role and orchestration rules of the originating project. Original documents retained unchanged as historical records. |
| 1.0.0 | 2026-10-24 | Approved by human instruction; content unchanged from draft. |
| 1.1.0 | 2026-10-29 | Generic template: remove originating-project runtime assumptions; document OpenCode V2 wiring and native enforcement limits; §2 splits roles, authorization and instruction sources; Worker produces a handoff, Coordinator owns the document pipeline. |
