---
name: Delegate Task
description: Create an isolated Git worktree and launch one worker subagent for a single assigned task
---

# Delegate a task to a worker

Use this skill before any implementation edit. Workers must not share the Coordinator checkout.

## Preconditions

1. The FSPEC is `In Progress` with recorded human start authorization.
2. Read `config/agent-policy.yaml` `workers.max_parallel`. Count every active or reserved implementation Worker session across the project, regardless of FSPEC. A session reserves a slot from the start of its bootstrap turn until it finishes or is cancelled. Start this delegation only if adding its reserved slot stays within the limit. If the project-wide count is unknown, do not start another Worker until all known active Workers finish.
3. The task has a file scope, dependencies, and acceptance-criteria references. Overlapping-file tasks stay sequential.
4. Parallelization reasoning is written in the task plan when more than one worker will run.
5. The Coordinator checkout is on the FSPEC integration branch recorded in the task plan. Stop if the current branch, recorded branch or authorized FSPEC version does not match.

## Worktree and session

1. Create a worktree from the recorded FSPEC integration branch with OpenCode `worktree_create`. Name it after the task ID (for example `TASK-0401`). Destination comes from `worktree.directory` in OpenCode config (`.worktrees` in this template).
2. Launch the `worker` subagent in the foreground with a bootstrap-only prompt containing the task ID and target worktree path. Instruct it to use no tools, perform no implementation work and return only `READY_FOR_MOVE`.
3. Capture the child `sessionID` after the bootstrap call completes. Stop if no child session ID is returned or the response reports any work.
4. Move that child session into the worktree with `session_move`.
5. Do not continue the child in the same turn as the bootstrap. A later human message must ask to continue that worker before you pass its `sessionID` to `subagent` with the complete Worker prompt below. That later call is the first call allowed to inspect or modify task files. Until then the plan stays `In Progress`.
6. Stop and report the blocker if the child cannot be moved or resumed. Do not fall back to implementation in the Coordinator checkout.
7. Native subagent context isolation is not filesystem isolation. Do not skip the worktree.

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
