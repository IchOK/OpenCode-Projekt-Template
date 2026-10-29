---
name: Delegate Task
description: Create an isolated Git worktree and launch one worker subagent for a single assigned task
---

# Delegate a task to a worker

Use this skill before any implementation edit. Workers must not share the Coordinator checkout.

## Preconditions

1. The FSPEC is `In Progress` with recorded human start authorization.
2. Read `config/agent-policy.yaml` `workers.max_parallel`. Count worker sessions already running for this FSPEC. If the count would exceed the limit, or the count is unknown, run sequentially.
3. The task has a file scope, dependencies, and acceptance-criteria references. Overlapping-file tasks stay sequential.
4. Parallelization reasoning is written in the task plan when more than one worker will run.

## Worktree and session

1. Create a worktree with OpenCode `worktree_create`. Name it after the task ID (for example `TASK-0401`). Destination comes from `worktree.directory` in OpenCode config (`.worktrees` in this template).
2. Launch the `worker` subagent only (`subagent` tool, agent ID `worker`).
3. Move that child session into the worktree with `session_move` before it edits files. If `session_move` cannot target the child, put the worktree path in the worker prompt and instruct the worker to move itself; if it still cannot, stop and report the blocker.
4. Native subagent context isolation is not filesystem isolation. Do not skip the worktree.

## Worker prompt (English)

Include all of:

- Task ID, FSPEC ID and version
- Worktree directory and branch
- File scope and forbidden paths
- Acceptance criteria IDs
- Required tests
- Instruction to return the structured handoff from the worker agent file

## After the worker returns

Review against the FSPEC and ADRs. Do not merge yet; use the `integrate-task` skill. Do not implement or patch production code yourself.
