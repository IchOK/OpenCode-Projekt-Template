# OpenCode multi-agent project template

Reusable project skeleton for [OpenCode](https://opencode.ai/v2/docs) V2: one Coordinator (primary agent) and isolated Worker subagents, with human-approved requirements, functional specifications, and ADRs.

This repository is a **template**, not an application. Copy it, then fill `project/project.md`.

## Language

| Surface | Language |
|---|---|
| Project overview (`project/project.md`, `docs/templates/project.md`, `project/project-example.md`) | German (body). YAML keys stay English. The Coordinator must not rewrite these into English. |
| Coordinator talking to the human | German |
| AGENTS.md, process docs, other templates, agent/skill files | English |
| REQ / FSPEC / ADR / TASK / IMP / execution logs | English |

## What OpenCode loads

| Path | Role |
|---|---|
| `AGENTS.md` | Persistent agent instructions (auto-discovered) |
| `.opencode/opencode.jsonc` | `default_agent`, extra `instructions`, worktrees, permissions |
| `.opencode/agents/coordinator.md` | Primary agent |
| `.opencode/agents/worker.md` | Implementation subagent |
| `.opencode/skills/` | `start-fspec`, `delegate-task`, `integrate-task`, `implementation-report` |
| `.opencode/commands/` | `/start-fspec`, `/delegate-task`, `/integrate-task`, `/implementation-report` |
| `config/agent-policy.yaml` | Limit **values** (injected as instructions, not enforced by OpenCode) |

The built-in `build` agent is **disabled** in project config so sessions start as the Coordinator. Re-enable it only if you accept bypassing this workflow:

```jsonc
"agents": { "build": { "disabled": false } }
```

## Enforced vs stated intent

OpenCode **can** restrict which agent is default, which subagent may be launched, which paths the Coordinator may edit, and where worktrees are created.

OpenCode **cannot** natively cap how many `worker` sessions run at once, count retries, or stop a human from switching to a non-disabled agent in the UI. Shell can bypass `edit` rules. See `docs/process/multi-agent-orchestration.md` §7.

## Workflow

1. Fill the German project overview in `project/project.md`.
2. Write and human-approve REQ → FSPEC → ADR (English).
3. Coordinator may set the FSPEC task plan to `Ready to Start`.
4. Implementation starts only on an explicit human instruction naming FSPEC ID and version (`/start-fspec`).
5. Each worker gets a Git worktree under `.worktrees/` before editing.
6. Coordinator integrates, verifies, writes `docs/implementation/IMP-FSPEC-….md`, then **waits**. Completing one FSPEC does not start the next.

Process rules: `docs/process/document-lifecycle.md` (PROC-0001) and `docs/process/multi-agent-orchestration.md` (PROC-0002). Those documents are at **1.1.0 Draft** until you re-approve them.

## Directory map

```text
AGENTS.md
README.md
config/agent-policy.yaml
.opencode/                 OpenCode V2 agents, skills, commands, config
docs/process/              PROC-0001, PROC-0002
docs/templates/            REQ, FSPEC, ADR, TASK, IMP, …
docs/requirements/
docs/functional_specs/
docs/adr/
docs/tasks/                FSPEC task plans and TASK files
docs/implementation/
docs/execution/
docs/poc/evidence/
project/project.md         German overview of the active project
project/project-example.md Labelled example only
project/todo-register.md
```

`docs/hardware/`, `docs/external/`, and `vendor/` are optional.

## Example content

[`project/project-example.md`](project/project-example.md) is the only file that should carry illustrative domain examples. Do not copy origin-project specifics into `AGENTS.md`, process docs, or agent prompts.
