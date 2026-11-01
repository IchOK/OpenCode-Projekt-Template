---
name: Create project
description: Create a new project by copying this template to a sibling directory, then remove the verifier agent and the PoC directory from the copy only. Use when the human asks to create a new project from the template or invokes /create-project.
---

# Create a new project

Use this skill when the human asks to create a new project from this template, or invokes `/create-project`.

The source repository stays intact. Deletions apply only to the new copy.

## Preconditions

1. Resolve the project name from the human message. If it is missing, stop and ask in German for the project name only. Do not propose a name. Wait for the answer.
2. Accept the name only as a single directory name. Reject an empty name, `.`, `..`, and any name that contains `/`, `\`, `:`, `*`, `?`, `"`, `<`, `>`, or `|`. If the name is rejected, ask again in German and wait. Do not copy yet.
3. The source is the current session directory. The destination is the sibling directory formed by the parent of that directory and the project name. Do not use `shell` to resolve either path.
4. Use `read` on the destination. If it returns a directory listing, stop in German and leave it untouched. Do not overwrite or merge. A destination that `read` cannot open does not exist yet.

## Actions

Use OpenCode tools only. Do not use `shell`. If a tool is denied, stop and report that in German. Do not replace a denied tool with `shell`.

1. List the source files with `glob`. Set `hidden` so `.opencode/` and `.well-known/` are included, and set `limit` high enough to return every file. If the result is truncated, raise `limit` and list again. Skip `.git/`, `.worktrees/`, and `vendor/`.
2. Copy each remaining file with `read` on the source path and `write` on the same relative path under the destination. Do not write into the source.
3. In the destination only, delete `.opencode/agents/verifier.md` and every file under `docs/poc/` with `patch` (`*** Delete File`). One `patch` may delete several files. If `patch` is not available, stop in German.
4. Check with `read` before telling the human the project exists:
   - `read` opens the source `.opencode/agents/verifier.md` and the source `docs/poc/` directory.
   - `read` cannot open either path in the destination.
5. Tell the human in German the destination path, that the copy has no `.git` directory, and that the verifier agent file and `docs/poc/` were removed from the copy.

## Stop

- Never delete `.opencode/agents/verifier.md` or `docs/poc/` in the source repository.
- Do not remove verifier skills or commands. Do not edit `project/project.md`, process docs, or application code.
- Do not use `shell`, and do not initialize Git, commit, or push.
- After the confirmation, wait.
