---
name: Verifier check
description: Verifier only. Run exactly one named CHECK from §6. Do not update the repository, do not run a second CHECK, and do not run an NA entry.
---

# Run one check

Use this skill for `/verifier-check`, or when the human names exactly one id from §6 of `docs/poc/orchestration-verification.md`.

## Arguments

The id is one token of the form `CHECK-NN` from the human message or `$ARGUMENTS`. It must be a row in §6. If the id is missing, unknown, or more than one id is present, stop and ask in German which single CHECK to run. Do not guess and do not run a range. If the id is an `NA` id, stop in German and do not write an evidence file.

## Do not

Do not fetch, reset, clean, or remove worktrees. Do not run any other CHECK. Do not record an NA entry and do not write `docs/poc/evidence/NA-*.md`. A Passed result does not start the next row.

## Do

Read the plan scenario whose heading is that id.

1. Run `opencode --version` once and copy that string into the evidence.
2. For a §6 CHECK, decide whether each §5 file exists by reading it with the read tool, and only these paths: `docs/requirements/REQ-0901.md`, `docs/functional_specs/FSPEC-0901.md`, `docs/functional_specs/FSPEC-0902.md`, `docs/functional_specs/FSPEC-0903.md`, `poc_fixture/.gitkeep`. A missing file is one the read tool cannot open. Do not use `ls`, `pwd`, `find`, or `git ls-files` for this inventory. Create each missing file with the edit tool, then commit them once with the allowed `git add` and `git commit` commands. Do not edit them again during the check.
3. Obey that scenario's Given/When/Then. For a When bullet that contains a quoted user message or a slash command, run `opencode run --format json` in this repository. Do not pass `--agent`, `--model`, a host, or a port. The message is only that quoted sentence or slash command, with no text before or after it. Do not mention a check ID, `default_agent`, or the plan in that message. A When bullet that tells you to read or list files has no `opencode run`. One run per When bullet. CHECK-03 reuses the session id printed by its first run for its later When sentences. Every other check gets a new run.
4. Read the run's JSON for the Coordinator transcript, child sessions, model id, and token counts. Do not prompt a Worker session. Write only `docs/poc/evidence/<id>.md`.

Every later git command is `git -C <repo>` plus one subcommand from the verifier allowlist, one command per call. Use `grep -x` only for the oracle command named in the plan. Copy session ids, model ids, token counts, cost, and timestamps from the `opencode run` JSON or from git. Leave a field `null` when that output has no such value. Do not convert epoch numbers into clock times.

A check runs once. Do not start it again after a server restart, a rejected command, or a missing Then bullet. Do not switch model after a failure. Do not send a second prompt that tells the Coordinator to call an internal tool.

## Stop

A CHECK is Passed only when every Then bullet was observed. When any Then bullet is missing, or a Coordinator run or `git worktree list` shows a path outside this repository's `.worktrees/`, write the check as Failed. Do not delete that worktree and do not mark it Passed.

Report that one id and its status in German, then stop.
