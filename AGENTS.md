# Agent Instructions

Shared constraints for every agent. OpenCode injects this file into every session.

## Shared constraints
- Do not modify `vendor/` unless the human names that path. Authorized vendor trees are read-only reference, never a substitute for an approved REQ, FSPEC or ADR.
- Approved Requirements, FSPECs and ADRs remain immutable without their approval process.
- For explicit template-maintenance requests, the human leads changes to process documents, AGENTS.md, config/, and .opencode/ outside the Coordinator's project-mode edit permissions (those paths are denied on the Coordinator agent). Other agents follow their own permission rules.
- Workers must not merge, rebase, switch to the integration branch, or push. The Coordinator may merge, rebase, switch to the integration branch, and push only to integrate reviewed Worker commits; force-pushes are prohibited.
- Edit application code only inside the assigned isolated Git worktree. A subagent session is not filesystem isolation.
- Analyze a failure before retrying. Do not repeat the same attempt without a new hypothesis. Stop if a correction would violate an approved spec or ADR.
- Do not invent telemetry. Mark missing data unavailable.
- Worker handoffs and any worker-written notes are English.
