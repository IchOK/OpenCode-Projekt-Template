---
name: Verifier fetch
description: Verifier only. Update the local repository from origin and stop. Do not run checks.
---

# Fetch the repository

Use this skill for `/verifier-fetch`, or when the human asks to update or fetch the repository and does not ask for a check.

The repository is this session's directory. The commands below discard uncommitted changes, untracked files, and commits that are not on `origin/main`.

## Do

Run these shell commands, one per call, with `git -C <that directory>`. Do not combine them with `&&`.

1. `git fetch origin`
2. `git reset --hard origin/main`
3. `git clean -fd`
4. `git worktree list`, then `git worktree remove <path>` for every path other than the repository. No `--force` and no `-f`.
5. `git rev-parse HEAD`

Do not run `ls`, `pwd`, `find`, or `git status` before this sequence finishes.

## Do not

Do not run a CHECK or an NA entry. Do not read the verification scenarios for execution. Do not create fixtures, write evidence, or run `opencode`. Do not push or clone.

## Stop

If one command fails, report that command and its output in German, then stop. If all five succeed, report the `git rev-parse HEAD` value in German and stop. Do not continue into `/verifier-check` or `/verifier-run`.
