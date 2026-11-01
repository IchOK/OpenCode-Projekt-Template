---
name: Verifier run
description: Verifier only. Update the local repository, then run every CHECK in §6 order. Stop at the first failure.
---

# Fetch, then run every check

Use this skill for `/verifier-run`, or when the human says "Führe alle Tests aus" or "Verifiziere alle Checks". This repository has no unit-test runner.

## Do not

Do not stop after the fetch when the update commands succeeded. Do not skip, reorder, or repeat a CHECK. Do not search for `package.json`, a Makefile, or `*test*` files. Do not run or record an NA entry. Do not write `docs/poc/evidence/NA-*.md`.

## Do

1. Run the `verifier-fetch` sequence: `git fetch origin`, `git reset --hard origin/main`, `git clean -fd`, `git worktree list`, then `git worktree remove <path>` for every path other than the repository. No `--force` and no `-f`. Then `git rev-parse HEAD`. One command per call. Do not combine them with `&&`. If one of those commands fails, report it in German and stop. Do not start checks.
2. Run `opencode --version` once and copy that string into the evidence.
3. Decide whether each §5 file exists by reading it with the read tool, and only these paths: `docs/requirements/REQ-0901.md`, `docs/functional_specs/FSPEC-0901.md`, `docs/functional_specs/FSPEC-0902.md`, `docs/functional_specs/FSPEC-0903.md`, `poc_fixture/.gitkeep`. A missing file is one the read tool cannot open. Do not use `ls`, `pwd`, `find`, or `git ls-files` for this inventory. Create each missing file with the edit tool, then commit them once with the allowed `git add` and `git commit` commands. Do not edit them again during checks.
4. Run §6 from the first row through the last row, in the order written. For each CHECK, obey its Given/When/Then. For a When bullet that contains a quoted user message or a slash command, call the OpenCode `subagent` tool once, in the foreground. Set `agent` to `coordinator`, `background` to false, and `prompt` to only that quoted sentence or slash command. Do not pass `model`, a host, or a port. Omit `sessionID` except when CHECK-03 continues the child from its first call; never pass the parent session id. The `description` is one short line and does not name a check, `default_agent`, or the plan. Do not mention those in the prompt. A When bullet that tells you to read or list files has no `subagent` call. One call per When bullet. CHECK-03 reuses the child session id returned by its first call for its later When sentences. Every other check omits `sessionID`. Do not run `opencode run`. Do not start a Coordinator or Worker session from the shell. If the `subagent` call is rejected, do not replace it with a shell command. Read the `subagent` tool result for the Coordinator transcript, child sessions, model id, and token counts. Do not prompt a Worker session and do not call `subagent` with agent `worker`. Write that CHECK's evidence file before the next row.

Every later git command is `git -C <repo>` plus one subcommand from the verifier allowlist, one command per call. Use `grep -x` only for the oracle command named in the plan. Copy session ids, model ids, token counts, cost, and timestamps from the `subagent` tool result or from git. Leave a field `null` when that output has no such value. Do not convert epoch numbers into clock times.

A check runs once. Do not start it again after a server restart, a rejected command, or a missing Then bullet. Do not switch model after a failure. Do not send a second prompt that tells the Coordinator to call an internal tool.

## Stop

A CHECK is Passed only when every Then bullet was observed. When any Then bullet is missing, or a Coordinator run or `git worktree list` shows a path outside this repository's `.worktrees/`, write that check as Failed and stop the run. Do not delete that worktree, do not mark the check Passed, and do not start a later check.

A Passed CHECK starts the next §6 row. The closing report lists each CHECK that ran and its status, and names the check that stopped the run. Checks that were not started have no evidence file. Say the run is complete only when every §6 CHECK is Passed. NA entries are not part of the run.
