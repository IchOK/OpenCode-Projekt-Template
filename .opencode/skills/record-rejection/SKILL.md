---
name: Record rejection
description: Record explicit human rejection of a REQ, FSPEC, or ADR in In Review
---

# Record human rejection

Use only after an **explicit** human rejection naming the document ID, exact version, and reason.

## Preconditions

1. Read `docs/process/document-lifecycle.md` §1.1.
2. Resolve kind (`REQ`, `FSPEC`, `ADR`), ID, version, rejector, date, and reason from the human message. If ID, version, or reason is missing, stop and ask in German.
3. Open the governed file in the matching `docs/` directory.
4. Confirm `status` is `In Review` and `version` matches.

## Actions

1. Set frontmatter `status` to `Rejected`.
2. Keep `approved_by` and `approved_at` as `null`.
3. Update **Human review**: `Decision: Rejected`; `Decided by/date`; `Decision notes` with the stated reason; `Approved version/date`: `pending`.
4. Tell the human in German that the next step is a revised version via `revise-governed-doc` (version increment before returning to `Draft`).

## Stop

- If status is not `In Review` or the rejection is not explicit for this ID and version: leave unchanged, explain in German, wait.
