---
name: Integrate Task
description: Review a worker handoff, merge from its worktree into the integration branch, and verify tests
---

# Integrate a worker task

Use this skill after a worker returns a handoff. The Coordinator performs Git integration; workers must not merge or push.

## Review

1. Check the handoff: task ID, branch/commit, files changed, tests, failures, blockers.
2. Diff the worktree/branch against the authorized FSPEC version and approved ADRs.
3. Reject and re-delegate if the change exceeds file scope, edits governance documents, or needs a new approval.
4. Do not fix production application code yourself. Send corrections back to a worker in the same worktree, respecting the retry limit in `config/agent-policy.yaml`.
5. Read the base and FSPEC integration branches from the task plan. Confirm the Coordinator checkout is on the recorded FSPEC integration branch.

## Integrate

1. Merge or rebase onto the recorded FSPEC integration branch only from the reviewed commit.
2. Run or arrange the integration tests required by the FSPEC and `config/agent-policy.yaml`.
3. Mark the task `Completed` only after integration and verification succeed.
4. Record `result_commit` on the task and in the task plan.
5. With `logging.automatic: false` in `config/agent-policy.yaml` (template default), create or append a file under `docs/execution/` from `docs/templates/execution-log.yaml`. Fill fields from the Worker handoff, integration commit, and test results. Leave unknown telemetry as `null`; never invent session IDs, tokens, or costs.
6. Confirm the reviewed Worker commit is reachable from the FSPEC integration branch and the Worker worktree is clean.
7. If `integration.remove_verified_task_worktrees` is true, remove the clean Worker worktree with `git worktree remove <path>` (allowed shell). Do not pass `--force`. Do not use OpenCode `worktree_remove` — it checks `edit` on the worktree path, and the Coordinator denies `edit` under `.worktrees/` so task application files cannot be patched from the integration checkout. Preserve the task branch for traceability.

## Stop

- On test failure or spec mismatch: do not mark `Completed`; follow the failure policy.
- On blocked, failed, unintegrated or dirty work: preserve the worktree and report its path. Never use forced removal without explicit human approval.
- A normal push requires confirmation. Force flags and forced `+refspec` pushes are prohibited; ask the human to resolve a required non-fast-forward push. Use server-side branch protection when absolute enforcement is required.
