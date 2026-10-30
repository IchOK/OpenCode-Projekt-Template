---
id: PROJECT-0001
title: "Titel des aktiven Projekts"
version: 0.1.2
template_version: 0.3.0
created_at: 2026-10-24
modified_at: 2026-10-29
author: ""
modified_by: ""
status: Draft
approved_by: null
approved_at: null
supersedes: null
references:
  requirements: []
  functional_specs: []
  adrs: []
review_required: []
---

# Projektbeschreibung

> **Sprache:** Fließtext, Überschriften und Tabellenköpfe **Deutsch**. YAML-Feldnamen und Statuswerte bleiben englisch. Vorlage: [`../docs/templates/project.md`](../docs/templates/project.md). Der Coordinator schreibt diese Datei nicht auf Englisch um.

**Einordnung:** Dieses Dokument vermittelt Projektkontext und Entwicklungsziele.
Es ersetzt keine freigegebenen Anforderungen (REQ), Funktionsspezifikationen (FSPEC) oder Architekturentscheidungen (ADR).
Aussagen zu konkreten Bausteinen sind Planungsstand, keine unveränderlichen Regeln.
Typ PROJECT — `status` und optionale `approved_*`-Felder betreffen nur diese Übersicht ([`docs/process/document-lifecycle.md`](../docs/process/document-lifecycle.md) §1.2), nicht Code-Freigabe.

Die Projektbeschreibung ist auf Deutsch. Alle übrigen Projektdokumente
(REQ, FSPEC, ADR, Tasks, Reports, Logs) werden auf Englisch verfasst.

> Gliederung und Ausfüllhilfe: [`../docs/templates/project.md`](../docs/templates/project.md).
> Ausgefülltes Beispiel: [`project-example.md`](project-example.md).

## 1. Zweck und Vision

<!-- TODO -->

## 2. Ziele und Erfolgskriterien auf Projektebene

<!-- TODO -->

## 3. Geplante Anwendungsfälle

<!-- TODO -->

## 4. Systemübersicht

### Hardware

<!-- TODO -->

### Software

<!-- TODO -->

### Kommunikation

<!-- TODO -->

### Backend/Betrieb

<!-- TODO -->

## 5. Bedienung, Betrieb, Sicherheit

<!-- TODO -->

## 6. Projektgrenzen und Nichtziele

<!-- TODO -->

## 7. Geplante Meilensteine

| Phase | Inhalt | Nachweis (vorläufig) |
|---|---|---|
| M1 | | |

## 8. Offene Entscheidungen und Risiken

<!-- TODO -->

Siehe auch [`todo-register.md`](todo-register.md).

## 9. Entwicklungs- und Dokumentationsprozess

- Versionskontrolle und Hosting-Ort:
- [`docs/process/document-lifecycle.md`](../docs/process/document-lifecycle.md)
- [`docs/process/multi-agent-orchestration.md`](../docs/process/multi-agent-orchestration.md)
- [`AGENTS.md`](../AGENTS.md)
- Ablaufkette: Idee → freigegebenes Requirement → freigegebene FSPEC → **expliziter
  Implementierungsstart** → Tasks/Implementierung → Verifikation und Implementation Report.

> **Aktives Projekt:** Dieses Dokument beschreibt das aktive Projekt (PROJECT-0001). Die Vorlage für neue Projekte ist [`docs/templates/project.md`](../docs/templates/project.md).
- Parallelität: geregelt durch [`config/agent-policy.yaml`](../config/agent-policy.yaml) und PROC-0002. OpenCode deckelt `max_parallel` nicht nativ.
- Modellzuordnungen sind Laufzeitkonfiguration; eingesetzte Modelle werden je Task protokolliert.

## 10. Änderungshistorie

| Version | Datum | Änderung |
|---|---|---|
| 0.1.0 | 2026-10-24 | Projektübersicht aus der Vorlage angelegt. |
| 0.1.1 | 2026-10-29 | Sprache der Projektbeschreibung auf Deutsch; Verweise auf OpenCode-V2-Verdrahtung. |
| 0.1.2 | 2026-10-29 | Sprachhinweis für den Coordinator; Vorlage 0.3.0. |
