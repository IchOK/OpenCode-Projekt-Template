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

## Integrate

1. Merge or rebase onto the integration branch only from the reviewed commit.
2. Run or arrange the integration tests required by the FSPEC and `config/agent-policy.yaml`.
3. Mark the task `Completed` only after integration and verification succeed.
4. Record `result_commit` on the task and in the task plan.
5. Append an execution-log entry under `docs/execution/` from `docs/templates/execution-log.yaml`. Mark missing telemetry `null` / unavailable; never invent it.

## Stop

- On test failure or spec mismatch: do not mark `Completed`; follow the failure policy.
- `git push --force` / `git push -f` is denied. Ask the human if a non-fast-forward push is required.
