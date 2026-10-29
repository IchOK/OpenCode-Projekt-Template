---
name: Start FSPEC
description: Start implementing an approved FSPEC only after an explicit human instruction naming FSPEC ID and version
---

# Start FSPEC implementation

Use this skill when the human asks to start, begin, or authorize implementation of a functional specification.

## Preconditions

1. Read `docs/process/document-lifecycle.md` §2–§3 and `config/agent-policy.yaml`.
2. Resolve the FSPEC ID and version from the human instruction. If either is missing, stop and ask in German.
3. Read `docs/functional_specs/` for that ID. Confirm `status` is Approved and the version matches.
4. Confirm start authorization is explicit (FSPEC ID + version named). Approval, `Ready to Start`, an available task, or completion of another FSPEC is not authorization.

## Actions

1. Load or create the task plan at `docs/tasks/FSPEC-<id>.md` from `docs/templates/tasks-for-fspec.md`.
2. If implementation status is `Not Started`, the Coordinator may first set `Ready to Start` after checking prerequisites. Do not skip the human start instruction for `In Progress`.
3. Set implementation status to `In Progress` only after the explicit start instruction.
4. Record authorization identity, timestamp, FSPEC ID and version in the task plan (`Start authorization:`).
5. Do not edit application code. Delegate via the `delegate-task` skill.

## Stop

- If the FSPEC is not approved, the version mismatches, or authorization is inferred: leave status unchanged, explain in German, wait.
- Never start a second FSPEC because the first one finished.
