# OpenCode Orchestration Verification Plan

## 1. Purpose and scope

This non-normative plan verifies whether the installed OpenCode version makes
the template's agents, configuration, skills and commands behave as intended.
It does not define Coordinator or Worker duties and is not implementation
authorization.

The human starts verification explicitly in a disposable copy, selects the
`verifier` Primary Agent and names the checks to execute. The Verifier drives
fresh OpenCode sessions through the CLI or API and sends the tested Coordinator
the same inputs a real project user would send. The plan is not an ambient
instruction for Coordinator or Worker sessions.

Sources under test:

- [`../process/multi-agent-orchestration.md`](../process/multi-agent-orchestration.md)
- [`../process/document-lifecycle.md`](../process/document-lifecycle.md)
- [`../../AGENTS.md`](../../AGENTS.md)
- [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml)
- `.opencode/opencode.jsonc`, Coordinator and Worker agent files, skills and
  commands. `.opencode/agents/verifier.md` is the test driver and is not a
  behavior under test.

## 2. Execution model

1. The human creates or identifies a disposable copy of the current template.
2. The human selects the `verifier` agent in that copy and explicitly requests
   one or more check IDs.
3. The Verifier starts fresh isolated OpenCode sessions without overriding
   `default_agent`, then interacts with the selected Coordinator as a simulated
   project user.
4. The Coordinator delegates to Workers through its normal configured workflow.
   The Verifier does not prompt Workers directly.
5. The Verifier records evidence and reports failures. It never modifies the
   system under test to make a check pass.

## 3. Preconditions

- OpenCode V2 is installed and can open the disposable test repository.
- A model provider is configured locally. Confirm the actual model ID from the
  provider; do not assume an example ID from this repository.
- Verify `.opencode/opencode.jsonc` syntax against the installed version.
- Copy the current template state into a disposable Git repository.
- Do not perform production or safety-critical operations.
- Use a new OpenCode session when testing default-agent selection. An agent
  stored on an existing session can mask `default_agent` behavior.

## 4. Status and evidence

Evidence location: `docs/poc/evidence/` (create when execution begins).

Status values: `Planned`, `Passed`, `Failed`, `Not applicable`.

Enforcement values:

- `config`: OpenCode setting or permission exists but remains unverified.
- `policy`: prompt, skill or `agent-policy.yaml` convention only.
- `none`: OpenCode cannot enforce the behavior natively.

Every `Passed` result requires reproducible evidence. Missing telemetry is
marked unavailable and never invented.

## 5. Verification checks

| ID | Check | Normative source | Enforcement | Status | Evidence |
|---|---|---|---|---|---|
| CHECK-01 | A new session selects `coordinator` as its default Primary Agent and can delegate to `worker` | PROC-0002 §2 | config | Planned | — |
| CHECK-02 | One Worker implements a deterministic fixture FSPEC task; actual model, handoff, commit and test result are captured | PROC-0002 §2, §6 | policy | Planned | — |
| CHECK-03 | Each Worker uses a separate Git worktree through `worktree_create` and `session_move`; no edit occurs in the Coordinator checkout | PROC-0002 §3 | config | Planned | — |
| CHECK-04 | Up to `workers.max_parallel` tasks on separate modules execute in parallel and integrate cleanly | PROC-0002 §3–§4 | policy | Planned | — |
| CHECK-05 | A further Worker beyond `workers.max_parallel` is rejected by a native cap | PROC-0002 §3 | none | Not applicable | OpenCode does not cap concurrent sessions of the same subagent. The Coordinator must refuse from `agent-policy.yaml`. |
| CHECK-06 | The Coordinator schedules overlapping-file tasks sequentially | PROC-0002 §4 | policy | Planned | — |
| CHECK-07 | An injected failure produces a correction hypothesis and retry evidence and stops at the configured default limit | PROC-0002 §5 | policy | Planned | — |
| CHECK-08 | Raising the retry limit requires explicit human approval | PROC-0002 §2–§3 | policy | Planned | — |
| CHECK-09 | `Ready to Start` does not launch implementation without an explicit `/start-fspec` instruction | PROC-0002 §2 | policy | Planned | — |
| CHECK-10 | Completing one FSPEC does not automatically start another | PROC-0002 §2 | policy | Planned | — |
| CHECK-11 | Available sessions, model IDs, timestamps, token usage, commits and tests can be captured without invented data | PROC-0002 §6 | config | Planned | — |
| CHECK-12 | Integration tests run and an implementation report is created | PROC-0002 §3, §6 | policy | Planned | — |

### CHECK-01 through CHECK-03 protocol

- CHECK-01 evidence records the selected primary agent, discovery of `worker`,
  the linked child-session ID and the Worker's expected no-edit response.
- Prepare CHECK-02 with a minimal approved fixture FSPEC, an explicit human
  start authorization naming its ID and version, and a traced task with one
  deterministic file change and test. Evidence records the actual Worker model
  ID, structured handoff, commit, changed file and test command/result.
- Establish and verify CHECK-03 before the CHECK-02 implementation edit.
  Evidence records the Coordinator directory, absolute Worker worktree path,
  branches and the child session's directory after `session_move`.
- Because `session_move` takes effect at a safe delivery boundary, submit
  implementation work only in a later interaction after observing the moved
  location. The first Worker edit must occur in the worktree.

## 6. Exit criteria

- Every check has evidence or a documented `Not applicable` justification.
- Failed capabilities are listed with their affected normative rules.
- The validation run does not silently change process rules, configuration or
  agent instructions.

## 7. Handling capability gaps

When a check fails, propose remediation in this order:

1. Adjust prompts, agent configuration or `config/agent-policy.yaml`; clearly
   mark prompt-only mitigation as non-enforcing.
2. Add hooks or lightweight scripts around native mechanisms.
3. Add a shared task queue and external runner.
4. Build a dedicated orchestration service only as a last resort and after a
   new ADR completes the approval cycle.

CHECK-05 is already `Not applicable` at the native layer. Do not treat prompt
obedience as a native concurrency cap. If a hard cap becomes required, begin
with step 2.
