---
name: Revise governed document
description: Return a governed REQ, FSPEC, or ADR to Draft with a new version after rejection or an approved revision instruction
---

# Revise governed document

Use when the human asks to revise after rejection, to apply review changes (return from `In Review` to `Draft`), or to start a new draft revision of an already **Approved** document.

## Preconditions

1. Read `docs/process/document-lifecycle.md` §1.1.
2. Resolve kind, ID, current version, and the human intent from the message:
   - **From `Rejected`:** version increment required before `Draft`.
   - **From `In Review` (changes requested, not rejected):** set `Draft`; increment version if the human asks for a new version line.
   - **From `Approved` (new revision):** human must name the approved ID and version explicitly; increment version; new `Draft`; do not mutate the prior approved Git revision.
3. Open the file in the correct `docs/` directory. Confirm current `status` matches the intended transition.

## Actions

1. Bump frontmatter `version` (semver patch by default unless the human names the next version).
2. Set `status` to `Draft`.
3. Set `approved_by` and `approved_at` to `null`.
4. Reset **Human review** to pending decision lines appropriate for a new draft cycle.
5. Apply content edits the human requested in the same instruction. Do not start implementation.

## Stop

- If an `Approved` revision is requested without naming the approved ID and version, or a `Draft` → `Approved` shortcut is implied: stop and explain in German.
- An approved version never returns to `Draft` in place; only a new version row in Git history via this revision flow.
