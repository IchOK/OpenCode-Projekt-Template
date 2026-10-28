---
id: PROC-0002
title: "Multi-Agent Orchestration"
status: Approved
version: 1.0.0
date: 2026-10-24
approved_by: "Projektverantwortlicher (human instruction, 2026-10-24)"
approved_at: 2026-10-24
---

# Multi-Agent Orchestration

## 1. Scope and applicability

- Applies to every project started on this template, not only the originating one.
- Execution model: one Coordinator (OpenCode Primary Agent) and up to the
  configured maximum number of Workers (native Subagents) for implementation of
  human-approved FSPECs.
- No custom orchestration service is built unless §8 determines native means
  demonstrably insufficient.
- The runtime model (e.g. a Qwen Flash Next endpoint) is a runtime choice and
  never part of this rule set (§9).

## 2. Roles and authorization

- **Coordinator**: plans, assigns, reviews, integrates, arranges integration
  tests and maintains documentation. Never implements or corrects production
  application code. Runs the §7 verification (PoC) in a disposable repository;
  scripts written solely for tool verification do not count as production code.
- **Workers**: implement, test and document assigned tasks only, each in an
  isolated Git worktree. Never merge into the integration branch; never modify
  approved governance documents.
- Binding agent instruction summary: [`../../AGENTS.md`](../../AGENTS.md).
- Implementation starts only on an explicit human start instruction naming the
  FSPEC ID and version. Approval, `Ready to Start`, an available task or the
  completion of another FSPEC never substitute for it
  ([`document-lifecycle.md`](document-lifecycle.md) §2–§3). The concrete workflow
  chain of the active project is documented in its project description, §9:
  [`../../project/project.md`](../../project/project.md).
- A completed FSPEC does not authorize the next one.

## 3. Technical constraints

Normative rules below; the concrete numbers are maintained centrally in
[`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) as the single
source of truth. Enforcement status of each rule is tracked in §7.

- Maximum concurrent implementation Workers, as maintained centrally in
  [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) (single source
  of truth); the Coordinator is not counted. This is a ceiling, not a target.
- Each Worker requires its own isolated Git worktree/branch before any edit;
  native Subagent sessions alone do not establish filesystem isolation.
- Default maximum of correction attempts per issue, as maintained centrally in
  [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml); any increase
  needs explicit human approval, recorded with rationale and new limit.
- Integration requires passing tests, documentation and spec compliance.

## 4. Parallelization

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

- Automatically capture where available: task ID, agent/role, actual model
  identifier, task status, integrated commits, test outcomes, failure reasons.
- Optionally capture calls, session IDs, timestamps, duration, tokens and costs
  when inexpensive. Missing data is marked unavailable, never invented, and must
  not block completion.
- Keep the detailed execution log separate from the concise implementation
  report (templates:
  [`execution-log.yaml`](../templates/execution-log.yaml),
  [`implementation.md`](../templates/implementation.md)).
- If a task uses multiple runs or models, preserve each run.

## 7. Verification of the orchestration (PoC)

The capabilities this rule set depends on are not yet proven against the
installed OpenCode version. Until each check below shows evidence, the affected
rule is **stated intent, not enforced fact**, and must be treated as such in
task plans and reports.

### 7.1 Preconditions

- OpenCode runs on the Mac; the DGX Spark exposes the vLLM OpenAI-compatible
  endpoint.
- Confirm the actual model ID from `/v1/models`; do not assume the repository's
  example ID. Verify OpenCode configuration syntax against the installed version.
- Tests run in a disposable Git repository; no real robot movement and no
  safety-critical operation at any point.

### 7.2 Verification checks

Evidence location: `docs/poc/evidence/` (create when runs begin) — governed by the binding directory structure in [`document-lifecycle.md`](document-lifecycle.md) §12. Status
values: `Planned`, `Passed`, `Failed`, `Not applicable`.

| ID | Check | Normative ref | Status | Evidence |
|---|---|---|---|---|
| CHECK-01 | Coordinator loads as default Primary Agent and can delegate to a Worker | §2 | Planned | — |
| CHECK-02 | One Worker implements a trivial FSPEC task; model and result captured | §2, §6 | Planned | — |
| CHECK-03 | Separate Git worktree/branch per Worker; verify no shared working directory | §3 | Planned | — |
| CHECK-04 | The configured maximum of tasks on separate modules execute in parallel and merge cleanly | §3, §4 | Planned | — |
| CHECK-05 | Task beyond the configured maximum is rejected: technical enforcement, not prompt obedience | §3 | Planned | — |
| CHECK-06 | Overlapping-file tasks are scheduled sequentially by the Coordinator | §4 | Planned | — |
| CHECK-07 | Injected failure: correction hypothesis, retry evidence, cap at configured default | §5 | Planned | — |
| CHECK-08 | Limit increase requires explicit human approval | §2, §3 | Planned | — |
| CHECK-09 | `Ready to Start` does not launch implementation without explicit human command | §2 | Planned | — |
| CHECK-10 | No automatic transition to a second FSPEC | §2 | Planned | — |
| CHECK-11 | Sessions, model IDs, timestamps, token usage, commits and tests available for automatic logs | §6, §7.1 | Planned | — |
| CHECK-12 | Integration tests run and an implementation report is created | §3, §6 | Planned | — |

### 7.3 Exit criteria

- Every check shows evidence or a documented `Not applicable` justification.
- Unmet capabilities are listed and trigger the escalation ladder in §8.
- Any unmet capability may require revising this document's rules through human
  approval; the PoC itself never changes §1–§6 silently.

## 8. Handling capability gaps

When a check fails, escalate in this order and document the decision:

1. Adjust prompts, agent configuration or `config/agent-policy.yaml`
   (non-enforcing — acceptable only as temporary measure, clearly marked).
2. Hooks and lightweight scripts around the native mechanisms.
3. Shared task queue and external runner.
4. Dedicated orchestration service — last resort; needs a new ADR through the
   approval cycle of [`document-lifecycle.md`](document-lifecycle.md) §1.

The superseded decision record ADR-0001 remains as historical rationale; it is
not edited.

## 9. Tool independence

- Task, implementation report and execution-log formats stay tool-independent.
- FSPECs never contain model assignments; model-to-role mapping and provider
  endpoints belong in runtime configuration.
- OpenCode is the current platform choice, not a permanent commitment; changes
  follow the ADR lifecycle.

## 10. Approval and history

Approver: Projektverantwortlicher (explicit human instruction, 2026-10-24)
Approved version/date: 1.0.0 / 2026-10-24

| Version | Date | Change |
|---|---|---|
| 0.1.0 | 2026-10-24 | Initial draft: consolidates ADR decision content and the PoC verification plan of the originating project. Original documents retained unchanged as historical records. |
| 1.0.0 | 2026-10-24 | Approved by human instruction; content unchanged from draft. |
