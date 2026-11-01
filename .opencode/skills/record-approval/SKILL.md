---
name: Record approval
description: Record explicit human approval of a REQ, FSPEC, or ADR in In Review (Approved state)
---

# Record human approval

Use only after an **explicit** human approval that names the document ID and exact version (for example: "Approve REQ-0003 v1.0.0 as Max, 2026-10-30").

## Preconditions

1. Read `docs/process/document-lifecycle.md` §1.1–§2.
2. Resolve kind (`REQ`, `FSPEC`, `ADR`), ID, version, approver name, and approval date/time only from explicit human messages in this approval. Do not take a missing ID, version, or name from an earlier turn, the open file, or whichever document is currently `In Review`. If ID, version, or name is missing, stop, leave the file unchanged, and ask in German for exactly one missing item, in this order: first the ID, then the version, then the name. Do not ask for a later item while an earlier one is still missing, and do not ask for more than one item in the same question. Wait for the answer, then repeat this check. Do not propose or prefill the name.
3. Open the file under `docs/requirements/`, `docs/functional_specs/`, or `docs/adr/` as appropriate.
4. Confirm `status` is `In Review` and `version` matches. There is no direct `Draft` → `Approved` path.

## Actions

1. Set frontmatter `status` to `Approved`.
2. Set `approved_by` and `approved_at` (ISO-8601 date or date-time) in frontmatter.
3. Update **Human review**: `Decision: Approved`; `Decided by/date`; `Approved version/date` matching the approved version; brief `Decision notes` if the human supplied any.
4. For FSPEC: remind in German that approval does **not** authorize implementation; `/start-fspec` with ID and version is still required.
5. Do not call `start-fspec`, create task worktrees, or delegate to workers.

## Stop

- If the human has not explicitly named this ID, this exact version, and the approver name, or status is not `In Review`: leave unchanged, explain in German, wait. A name alone is not an approval.
- Do not infer approval from silence, milestones, implementation readiness, or from a document that was just submitted for review.
