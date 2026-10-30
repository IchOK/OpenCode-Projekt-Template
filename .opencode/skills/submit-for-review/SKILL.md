---
name: Submit for review
description: Move a complete REQ, FSPEC, or ADR draft from Draft to In Review for human decision
---

# Submit governed document for review

Use when the Coordinator (or human) asks to submit a requirement, functional specification, or ADR for human review.

## Preconditions

1. Read `docs/process/document-lifecycle.md` §1.1.
2. Resolve document kind (`REQ`, `FSPEC`, or `ADR`), document ID, and exact version from the human instruction. If any is missing, stop and ask in German.
3. Open the file:
   - `REQ` → `docs/requirements/<ID>.md`
   - `FSPEC` → `docs/functional_specs/<ID>.md`
   - `ADR` → `docs/adr/<ID>.md`
4. Confirm frontmatter `status` is `Draft` and `version` matches the named version. Confirm `approved_by` and `approved_at` are `null`.
5. Confirm the body is complete enough for review (no empty mandatory sections). If not, stop and ask in German.

## Actions

1. Set frontmatter `status` to `In Review`.
2. Update the **Human review** section: `Decision: submitted for review`; record submitter and date; clear any stale approval lines while keeping `Approved version/date` as `pending` until a decision.
3. Do not change approved content scope beyond typos the human already requested in this turn.
4. Tell the human in German that the document awaits an explicit approval or rejection naming the same ID and version. Do not start FSPEC implementation.

## Stop

- If status is not `Draft`, version mismatches, or the transition would skip `In Review`: leave unchanged, explain in German, wait.
- Never set `Approved` or `Rejected` in this skill.
