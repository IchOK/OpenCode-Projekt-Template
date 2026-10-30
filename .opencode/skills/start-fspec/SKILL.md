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
5. Read `integration.base_branch` and `integration.fspec_branch_prefix` from `config/agent-policy.yaml`. Confirm the configured base branch exists.
6. **Single active FSPEC:** List versioned task plans in `docs/tasks/` (`FSPEC-*-v*.md`). If any plan other than this FSPEC's target file has `Implementation status: In Progress`, stop in German and name the blocking plan. If this plan is already `In Progress` for the same ID and version, continue (resume). Do not set a second plan to `In Progress`.

## Actions

1. Load or create the task plan at `docs/tasks/FSPEC-<id>-v<version>.md` (for example `FSPEC-0004-v1.0.0.md`) from `docs/templates/tasks-for-fspec.md`. Use the **authorized version from the start instruction** in the filename. Do not reuse or overwrite a plan file for a different version.
2. Derive the integration branch as `<fspec_branch_prefix><FSPEC-ID>-v<version>` (for example `fspec/FSPEC-0004-v1.0.0`). Create it from the configured base branch, or verify that an existing branch is recorded for this exact FSPEC ID and version.
3. Record the base branch and FSPEC integration branch in the task plan. Switch the Coordinator checkout to that FSPEC integration branch before delegation.
4. If implementation status is `Not Started`, the Coordinator may first set `Ready to Start` after checking prerequisites. Do not skip the human start instruction for `In Progress`.
5. Set implementation status to `In Progress` only after the explicit start instruction.
6. Record authorization identity, timestamp, FSPEC ID and version in the task plan (`Start authorization:`).
7. Do not edit application code. Delegate via the `delegate-task` skill.

## Stop

- If the FSPEC is not approved, the version mismatches, authorization is inferred, or the base/integration branch is ambiguous: leave status unchanged, explain in German, wait.
- Never start a second FSPEC because the first one finished.
- Never set `In Progress` while another versioned task plan is already `In Progress` (§3 single active FSPEC).
