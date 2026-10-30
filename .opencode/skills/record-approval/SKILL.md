---
name: Record approval
description: Record explicit human approval of a REQ, FSPEC, or ADR in In Review (Approved state)
---

# Record human approval

Use only after an **explicit** human approval that names the document ID and exact version (for example: "Approve REQ-0003 v1.0.0 as Max, 2026-10-30").

## Preconditions

1. Read `docs/process/document-lifecycle.md` §1.1–§2.
2. Resolve kind (`REQ`, `FSPEC`, `ADR`), ID, version, approver identity, and approval date/time from the human message. If ID, version, or approver is missing, stop and ask in German.
3. Open the file under `docs/requirements/`, `docs/functional_specs/`, or `docs/adr/` as appropriate.
4. Confirm `status` is `In Review` and `version` matches. There is no direct `Draft` → `Approved` path.

## Actions

1. Set frontmatter `status` to `Approved`.
2. Set `approved_by` and `approved_at` (ISO-8601 date or date-time) in frontmatter.
3. Update **Human review**: `Decision: Approved`; `Decided by/date`; `Approved version/date` matching the approved version; brief `Decision notes` if the human supplied any.
4. For FSPEC: remind in German that approval does **not** authorize implementation; `/start-fspec` with ID and version is still required.
5. Do not call `start-fspec`, create task worktrees, or delegate to workers.

## Stop

- If the human message does not explicitly approve this ID and version, or status is not `In Review`: leave unchanged, explain in German, wait.
- Do not infer approval from silence, milestones, or implementation readiness.
