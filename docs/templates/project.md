---
id: PROJECT-XXXX
title: "Projekttitel"
version: 0.1.0
template_version: 0.3.0
created_at: YYYY-MM-DD
modified_at: YYYY-MM-DD
author: ""
modified_by: ""
status: Draft
approved_by: null
approved_at: null
supersedes: null
references:
  requirements: [REQ-XXXX, ...]
  functional_specs: [FSPEC-XXXX, ...]
  adrs: [ADR-XXXX, ...]
review_required: []
---

# Projektbeschreibung

> **Sprache (verbindlich für Coordinator und Menschen):** Der gesamte Fließtext, alle Überschriften und Tabellenköpfe dieser Datei sind **Deutsch**. Das gilt auch für die aktive Übersicht [`../../project/project.md`](../../project/project.md) und das Beispiel [`../../project/project-example.md`](../../project/project-example.md). YAML-Feldnamen und Statuswerte (`Draft`, `Approved`, …) bleiben englisch — das sind Prozess-IDs, keine Fließtextsprache. Der Coordinator darf die Projektbeschreibung **nicht** ins Englische umschreiben.

**Einordnung:** Dieses Dokument vermittelt Projektkontext und Entwicklungsziele. Es ersetzt keine freigegebenen Anforderungen (REQ), Funktionsspezifikationen (FSPEC) oder Architekturentscheidungen (ADR). Aussagen zu konkreten Bausteinen sind Planungsstand, keine unveränderlichen Regeln.

Anforderungen, Funktionsspezifikationen, ADRs, Tasks, Implementation Reports und Execution Logs werden auf **Englisch** verfasst. Nur die Projektbeschreibung ist Deutsch.

## 1. Zweck und Vision

Beschreibe das Problem, das grobe Systemkonzept (wichtige Teilsysteme und ihr Zusammenhang), die vorgesehene Betriebsumgebung und den Entwicklungsansatz (z. B. gestufte Nachweise statt einmaliger Komplettlieferung). Halte fest, wie spätere Erweiterung gedacht ist.

## 2. Ziele und Erfolgskriterien auf Projektebene

Liste die Ziele als messbare Aussagen (Zielwert, Toleranz, Betriebsbedingungen). Kennzeichne alles, was noch nicht formalisiert ist. Halte ausdrücklich fest, dass Ziele auf dieser Ebene **noch keine formal freigegebenen Einzelanforderungen** sind; Messmethoden, Toleranzen und Abnahmekriterien stehen in den englischen REQ- und FSPEC-Dokumenten.

## 3. Geplante Anwendungsfälle

Liste die vorgesehenen Szenarien als kurze nummerierte Anwendungsfälle: wer betreibt das System, unter welchen Bedingungen, und welches Verhalten gilt als Erfolg.

## 4. Systemübersicht

Beschreibe die großen Teilsysteme in Unterabschnitten, wo sinnvoll je eines:

- **Hardware:** ausgewählte Komponenten und Varianten, Betriebsorte, Plattformwahl und ggf. geplanter Plattform-Prüfpfad.
- **Software:** Sprache/Laufzeit je Baustein; begründe die Wahl und verweise auf das zugehörige ADR.
- **Kommunikation:** Pfade und Protokolle zwischen Teilsystemen, einschließlich der zu transportierenden Daten und der zu prüfenden Interoperabilität.
- **Backend/Betrieb:** Rolle einer zentralen Planungs-, Monitoring- oder Logging-Komponente; betriebliche Grenzen je Baustein.

Jede unbestätigte Auswahl als noch zu bestätigen kennzeichnen, nicht als fest darstellen.

## 5. Bedienung, Betrieb, Sicherheit

Beschreibe das Betriebskonzept: welche menschliche Freigabe vor dem Betrieb nötig ist, wie Zustände gemeldet werden, definiertes Stopp- oder Fallback-Verhalten bei Störungen, Berechtigungen für Updates/Deployments, und welche Maßnahmen nachgewiesen sind versus welche noch Entwicklungsansatz sind.

## 6. Projektgrenzen und Nichtziele

Liste ausdrücklich, was der erste Umfang ausschließt. Der Abschnitt verhindert, dass Ziele als stille Zusagen gelesen werden.

## 7. Geplante Meilensteine

| Phase | Inhalt | Nachweis (vorläufig) |
|---|---|---|
| M1 | <Meilensteinziel> | <beobachtbarer Nachweis> |
| M2 | <Meilensteinziel> | <beobachtbarer Nachweis> |

Halte fest, dass Reihenfolge und Abnahmekriterien durch freigegebene Spezifikationen konkretisiert werden und die Tabelle eine Roadmap ist, kein Implementierungsauftrag.

## 8. Offene Entscheidungen und Risiken

Liste offene Fäden, nach Art gruppiert wo hilfreich. Verweise auf das Todo-Register für Punkte, die unterwegs entstehen. Neue verbindliche Entscheidungen folgen den Zyklen für ADR, Anforderungen und Funktionsspezifikationen — nicht diesem Dokument.

## 9. Entwicklungs- und Dokumentationsprozess

- Versionskontrolle und Hosting-Ort.
- Regelwerke: [`../process/document-lifecycle.md`](../process/document-lifecycle.md), [`../process/multi-agent-orchestration.md`](../process/multi-agent-orchestration.md).
- Verbindliche Agentenanweisungen: [`../../AGENTS.md`](../../AGENTS.md).
- Ablaufkette des aktiven Projekts: [`../../project/project.md`](../../project/project.md) §9.

> **Hinweis Vorlage:** Dieses Dokument ist die Vorlage für ein neues Projekt; es beschreibt kein aktives Projekt. Die referenzierte Ablaufkette gehört zum aktiven Projekt.
- Parallelität: der Coordinator darf höchstens so viele Worker gleichzeitig nutzen, wie in [`../../config/agent-policy.yaml`](../../config/agent-policy.yaml) steht, unter den Bedingungen in PROC-0002. OpenCode deckelt diese Zahl nicht nativ.
- Modell- und Tool-Zuordnungen sind austauschbare Laufzeitkonfiguration; eingesetzte Modelle werden je Task protokolliert, soweit verfügbar.

## 10. Änderungshistorie

| Version | Datum | Änderung |
|---|---|---|
| 0.1.0 | YYYY-MM-DD | <Erstversion> |
