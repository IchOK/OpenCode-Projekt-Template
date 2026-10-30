# OpenCode multi-agent project template

Reusable project skeleton for [OpenCode](https://opencode.ai/v2/docs) V2: one Coordinator (primary agent) and isolated Worker subagents, with human-approved requirements, functional specifications, and ADRs.

This repository is a **template**, not an application. Copy it, then fill `project/project.md`.

## Template vs. project

| | Template (this repo) | Your derived project |
|---|---|---|
| Purpose | Reusable OpenCode multi-agent skeleton | One product or programme you build |
| `project/project.md` | Placeholder / German overview to fill | **Your** live project context |
| `docs/poc/`, `verifier` agent | Optional **template verification** tooling | Safe to **remove** if you do not run PoC checks |
| Process docs `PROC-*` | Copy and later approve for **your** project | Becomes **your** governance once approved |
| License files | [`LICENSE.md`](LICENSE.md), [`docs/legal/`](docs/legal/) | Keep or replace when you redistribute; update placeholders |

Implementation authority always comes from **your** approved FSPECs and explicit `/start-fspec` — not from copying the template.

## After copying (optional cleanup)

If the derived repo is **only** for product work (no template PoC):

- Remove or stop using [`.opencode/agents/verifier.md`](.opencode/agents/verifier.md) (select `coordinator` as Primary Agent).
- Remove [`docs/poc/`](docs/poc/) (verification plan and evidence), or keep a copy elsewhere for reference.
- Update [`LICENSE.md`](LICENSE.md).

## License and AI / TDM

| Path | Role |
|---|---|
| [`LICENSE.md`](LICENSE.md) | Summary: Apache-2.0 + [No-AI addendum](docs/legal/NO-AI-ADDENDUM.md) (code); CC BY 4.0 + EU TDM reservation (docs/text) |
| [`robots.txt`](robots.txt) | Opt-out signals for known AI crawlers when served as a website |
| [`.well-known/tdmrep.json`](.well-known/tdmrep.json) | [TDMRep](https://w3c.github.io/cg-reports/tdmrep/CG-FINAL-tdmrep-20240510/) reservation file (JSON **array**; update policy URL to your origin) |

This is not legal advice. Have a lawyer review terms before relying on them in production.

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
| `AGENTS.md` | Shared constraints only (auto-discovered for every agent) |
| `.opencode/opencode.jsonc` | `default_agent`, worktrees, permissions. No process `instructions`. |
| `.opencode/agents/coordinator.md` | Primary agent; tells the Coordinator to read PROC on demand |
| `.opencode/agents/worker.md` | Implementation subagent; Coordinator skills denied |
| `.opencode/agents/verifier.md` | Independently selected Primary Agent; drives disposable OpenCode sessions as a simulated user |
| `.opencode/skills/` | Coordinator-only: governance (`submit-for-review`, `record-approval`, `record-rejection`, `revise-governed-doc`) and implementation (`start-fspec`, `delegate-task`, `integrate-task`, `implementation-report`) |
| `.opencode/commands/` | `/submit-for-review`, `/record-approval`, `/record-rejection`, `/revise-governed-doc`, `/start-fspec`, `/delegate-task`, `/integrate-task`, `/implementation-report` |
| `config/agent-policy.yaml` | Limit **values**; Coordinator reads the file; not injected into the Worker |

The built-in `build` agent is **disabled** in project config so sessions start as the Coordinator. Re-enable it only if you accept bypassing this workflow:

```jsonc
"agents": { "build": { "disabled": false } }
```

## Enforced vs stated intent

OpenCode **can** restrict which agent is default, which subagent may be launched, which paths the Coordinator may edit, and where worktrees are created.

OpenCode **cannot** natively cap how many `worker` sessions run at once, count retries, or stop a human from switching to a non-disabled agent in the UI. Shell can bypass `edit` rules. Validation is defined separately in [`docs/poc/orchestration-verification.md`](docs/poc/orchestration-verification.md).

## Independent template verification

Template verification is separate from project work. In a disposable copy,
select the `verifier` Primary Agent and name the checks to run. It drives fresh
OpenCode sessions through the CLI or API and interacts with the Coordinator as
a simulated project user. Coordinator and Worker instructions do not load the
verification plan.

## Workflow

1. Fill the German project overview in `project/project.md`.
2. Write and human-approve REQ → FSPEC → ADR (English) via `/submit-for-review`, then `/record-approval` or `/record-rejection`; revise with `/revise-governed-doc` when needed.
3. Coordinator may set the FSPEC task plan to `Ready to Start`.
4. Implementation starts only on an explicit human instruction naming FSPEC ID and version (`/start-fspec`).
5. The Coordinator creates `fspec/<FSPEC-ID>-v<version>` from the configured
   base branch. Each Worker gets a task worktree under `.worktrees/`.
6. Reviewed tasks integrate into the FSPEC branch; clean verified task
   worktrees are removed without force.
7. After final verification, the FSPEC branch integrates into the base branch.
   The Coordinator writes `docs/implementation/IMP-FSPEC-<ID>-v<version>.md` and updates the matching task plan `docs/tasks/FSPEC-<ID>-v<version>.md`, then **waits**.
   Completing one FSPEC does not start the next.

Process rules: `docs/process/document-lifecycle.md` (PROC-0001) and `docs/process/multi-agent-orchestration.md` (PROC-0002). Those documents are at **1.1.0 Draft** until you re-approve them.

## Directory map

```text
AGENTS.md
LICENSE.md
README.md
robots.txt
.well-known/tdmrep.json
docs/legal/               LICENSE summary, No-AI addendum, TDM policy
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
docs/poc/orchestration-verification.md  Non-normative OpenCode behavior verification plan
docs/poc/evidence/
project/project.md         German overview of the active project
project/project-example.md Labelled example only
project/todo-register.md
```

`docs/hardware/`, `docs/external/`, and `vendor/` are optional.

## Example content

[`project/project-example.md`](project/project-example.md) is the only file that should carry illustrative domain examples. Do not copy origin-project specifics into `AGENTS.md`, process docs, or agent prompts.
