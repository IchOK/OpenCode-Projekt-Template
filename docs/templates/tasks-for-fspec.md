# Tasks for FSPEC-XXXX v1.0.0

Implementation status: Not Started
Start authorization: none

> Keep each task uniquely identified. The coordinator owns this plan. No human task-level approval is required, but an explicit human start authorization is mandatory before code changes.

> Status semantics: Task status (Open, In Progress, …) and FSPEC implementation status (Not Started, Ready to Start, …) are separate dimensions; they are not to be conflated.

## Numbering convention (seit 2026-10-21, dokumentiert — bestehende IDs bleiben unbenannt)

> Normative source: Task-IDs werden ausschließlich nach dieser Konvention vergeben; [`task.md`](task.md) verweist lediglich darauf.

- Der **Präfix kennzeichnet die Art**: `FSPEC-…` ist immer eine Spezifikation,
  `TASK-…` ist immer ein Task. Ein Zahlendreher wie `1102` kann nur noch als
  `FSPEC-…` oder `TASK-…` antreten, nie als beides.
- **Task-IDs tragen ihren FSPEC-Ursprung**: `TASK-<FF><n>`, `FF` = FSPEC-Nummer
  zweistellig, `n` = laufend im Plan. FSPEC-0004 → TASK-0401, 0402…; FSPEC-0009 →
  TASK-0901… (FSPEC ≥ 0010 wechselt auf `TASK-<fpec>-<n>`, z. B. TASK-0010-1,
  damit kein Lesekonflikt mit geerbten 100er-Blöcken.)
- **Historie unverändert**: FSPEC-0001 (0001…0003), FSPEC-0002 (1001…1003) und
  FSPEC-0003 (1101…1104) bleiben wie gewachsen — keine Hof-Möblierung durch
  unbenannte Referenzen in Branches, Evidenzen oder Logs.
- Ein Task-Plan für ein neues FSPEC beginnt bei `TASK-<FF>01`.

## Parallelization assessment
Dependencies, file overlaps, interfaces, expected merge effort, selected concurrency:

## TASK-0001 — Example
Status: Open  
Assigned to: unassigned  
Depends on: none  
Repository: esp32-mesh-framework-<gateway|nodes|node-red> (Code-Repo; Main-only tasks: none)  
Files scope: TBD  
Acceptance criteria: TBD  
Result commit: none <!-- bei Integration als repo@sha, z. B. esp32-mesh-framework-gateway@a1b2c3d (ADR-0003) -->

Description: TBD

Acceptance evidence: TBD
