---
name: Implementation Report
description: Write the FSPEC implementation report and notify the human in German that work is waiting for the next instruction
---

# Implementation report

Use this skill when every required task for the authorized FSPEC is integrated and verified on its recorded FSPEC integration branch.

## Finalize the FSPEC branch

1. Confirm every required task is `Completed`, all task commits are reachable from the recorded FSPEC integration branch, and every retained Worker worktree has a documented reason.
2. Review the complete FSPEC branch against the authorized FSPEC version and approved ADRs.
3. Switch to `integration.base_branch` from `config/agent-policy.yaml` and merge the reviewed FSPEC branch tip.
4. Run the final integration tests. On conflict, test failure or scope mismatch, do not complete the FSPEC; return to the FSPEC branch and re-delegate corrective application-code work.
5. Preserve the FSPEC branch for traceability.

## Report

1. On the base branch, create `docs/implementation/IMP-FSPEC-<id>-v<version>.md` from `docs/templates/implementation.md`, matching the **authorized FSPEC version** for this implementation run.
2. Fill summary, acceptance criteria with evidence, integrated tasks and commits, verification, ADRs, model provenance, limitations.
3. Link execution logs under `docs/execution/`.
4. Complete the checklist. Set implementation status to `Completed` on the versioned task plan at `docs/tasks/FSPEC-<id>-v<version>.md`.
5. Write the report in English.

## Notify and wait

1. Tell the human in German that the FSPEC is complete and that the next FSPEC will not start automatically.
2. Stop. Do not infer authorization for another FSPEC from this completion.
