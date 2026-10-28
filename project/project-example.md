# Projektbeschreibung — Beispieldatei

> **Dies ist ein Beispiel** für alle, die das Template zum ersten Mal ausfüllen.
> Es erklärt, was in die einzelnen Abschnitte gehört und wie man sie formuliert.
> Zu jedem Abschnitt gibt es nur kurze, allgemeine Beispiele.
> Die Gliederung entspricht exakt
> [`../docs/templates/project.md`](../docs/templates/project.md).
> Die eigentliche Projektübersicht steht je Projekt unter `./project.md`.

## 1. Zweck und Vision

**Was hier rein gehört:** Das Problem, das das Projekt löst, das grobe
Systemkonzept (welche Bauteile zusammenwirken), die angedachte Betriebsumgebung
und der eingeschlagene Entwicklungsansatz (z. B. schrittweise PoCs statt
Komplettdelivery). Ein Absatz, mehr nicht — Details stehen später in REQ/FSPEC.

**Beispiel (kurz):** „Ein Dienst, der Sensorwerte von Filialgeräten bündelt und
Ausfälle meldet. Entwicklung als aufeinander aufbauende Einzelfunktionen."

## 2. Ziele und Erfolgskriterien auf Projektebene

**Was hier rein gehört:** Die Ziele als messbare Aussagen (Zielwert, Toleranz,
Randbedingungen). Alles, was noch nicht als REQ formalisiert ist, entsprechend
kennzeichnen.

**Beispiel (kurz):**

| Ziel | Messgröße | Status |
|---|---|---|
| Ausfall eines Geräts erkannt | Alarm innerhalb ~60 s | noch nicht als REQ formalisiert |
| Betrieb mit vielen Teilnehmern | bis 50 Geräte gleichzeitig | Akzeptanz noch in FSPEC zu klären |

Wichtig: **Projektleitziele sind keine fertigen Anforderungen.** Messmethode,
Toleranzen und Abnahmekriterien entstehen in Requirements und Functional
Specifications.

## 3. Geplante Anwendungsfälle

**Was hier rein gehört:** Kurze, nummerierte Szenarien — wer betreibt, unter
welchen Bedingungen, und welches Verhalten markiert Erfolg.

**Beispiel (kurz):**
1. Ein Betreiber ruft die Sammelansicht auf und sieht alle Geräte im Normalzustand.
2. Ein Gerät meldet Ausbleibende Werte, das System quittiert den Alarm an die gleiche Stelle.

## 4. Systemübersicht

**Was hier rein gehört:** Die groben Bauteile, am besten in Unterabschnitten.
Jede unbestätigte Auswahl als „noch zu bestätigen" kennzeichnen, statt sie als
fest darzustellen.

- **Hardware:** eingesetzte Bauteile, Plattformwahl, ggf. geplante Plattformprüfung.
  *Beispiel (kurz): „Empfänger-/Sensoreinheit je Filiale, zentraler Server;
  Plattformvariante für späteren Betrieb noch gewählt."*
- **Software:** Sprache/Laufzeit je Baustein, mit begründeter Entscheidung und
  ADR-Verweis.
  *Beispiel (kurz): „Warum diese Laufzeit — weil Daten zwischen Knoten geteilt
  werden, siehe ADR-0003." Begründung wird referenziert, nicht bloß behauptet.*
- **Kommunikation:** welche Datenpfade bestehen, in welche Richtung, mit
  Interoperabilitäts-Lücken als offene Punkte.
  *Beispiel (kurz): „Gerät → Server (Werte), Server → Webansicht (Anzeige);
  Protokollwahl noch zu verifizieren."*
- **Backend/Betrieb:** Rolle einer zentralen Instanz, Betriebsgrenzen je
  Baustein, konfigurierbare Parameter.
  *Beispiel (kurz): „Ein Backend für Planung, Monitoring, Logging; je Gerät nur
  eine aktive Aufgabe."*

## 5. Bedienung, Betrieb, Sicherheit

**Was hier rein gehört:** Betriebskonzept — welche menschliche Freigabe braucht
den Betrieb, wie werden Zustände gemeldet, welches Abschalt- oder
Fallback-Verhalten gilt bei Störungen, und wie wird aktualisiert.
Besonders wichtig: klar zwischen **bewiesen** und **Entwicklungsansatz** trennen.

**Beispiel (kurz):** „Start je Mission nur nach bewusster Freigabe. Bei
Ausbleiben der Werte: Abbruch, kein automatisches Weitermachen. Geplante
Zwischenabschaltung ist ein **Entwicklungsansatz und noch nicht nachgewiesen**."

## 6. Projektgrenzen und Nichtziele

**Was hier rein gehört:** Ausdrücklich was der erste Umfang nicht enthält. Der
Abschnitt verhindert, dass gute Absichten als stille Zusagen gelesen werden.
Typische Kandidaten: volle Autonomie, nicht nachgewiesene Sicherheitszusage,
Zulassung/Produktreife, Festlegung auf bestimmte Tools oder Modelle.

**Beispiele (kurz):**
- Kein autonomer Betrieb ohne Beaufsichtigung.
- Kein Versprechen von Genauigkeit außerhalb definierter Bedingungen.
- Keine Festlegung auf ein bestimmtes LLM oder Framework.

## 7. Geplante Meilensteine

**Was hier rein gehört:** Phasen mit Inhalt und (vorläufigem) Nachweis.
Als Tabelle geeignet. Dazu der deutliche Hinweis, dass die Reihenfolge durch
freigegebene Spezifikationen konkretisiert wird — es ist eine **Planung, kein
Implementierungsauftrag**.

**Beispiel (kurz):**

| Phase | Inhalt | Nachweis (vorläufig) |
|---|---|---|
| M1 | Basis-Kommunikation | Nachricht nachweislich übertragen |
| M2 | Datenerfassung | Werte sichtbar und dauerhaft haltbar |
| M3 | Anzeige und Alarm | End-to-End-Test nachvollziehbar |

## 8. Offene Entscheidungen und Risiken

**Was hier rein gehört:** Offene Fäden, möglichst nach Art sortiert:
unklare Bauteilwahl, offene technische Machbarkeit, Betriebs- und
Sicherheitsrisiken, Rahmenbedingungen. Lockere Todos, die unterwegs entstehen,
stehen im Register, nicht in diesem Dokument. Neue verbindliche Entscheidungen
immer über ADR/REQ/FSPEC — nie stillschweigend.

**Beispiele (kurz):**
- Unklare Bauteile: Variante des Funkmoduls noch bestätigen.
- Machbarkeit: Zusammenspiel zweier Laufzeiten noch zu verifizieren.
- Prozess: Werden parallele Worker und automatisches Logging technisch erzwungen
  (→ [`docs/process/multi-agent-orchestration.md`](../docs/process/multi-agent-orchestration.md) §7).

## 9. Entwicklungs- und Dokumentationsprozess

**Was hier rein gehört:** Wo Code und Dokumentation liegen (VCS, Hoster),
Links zu den Regelwerken und die Ablaufkette — jeweils nur, ohne sie
nachzuerzählen.

**Beispiel (kurz):**

- Git-Repository bei `example-hoster.tld`, Details im Git.
- [`docs/process/document-lifecycle.md`](../docs/process/document-lifecycle.md) — Freigaben und Zustände.
- [`docs/process/multi-agent-orchestration.md`](../docs/process/multi-agent-orchestration.md) — Rollen, Grenzen, Verifikationsstand.
- [`AGENTS.md`](../AGENTS.md) — verbindliche Agenten-Anweisungen.
- Ablauf: Idee → REQ freigegeben → FSPEC freigegeben → **expliziter
  Startbefehl** → Tasks → Verifikation → Report.
- Modelle sind austauschbare Laufzeitkonfiguration; der Einsatz pro Task wird
  protokolliert.

## 10. Änderungshistorie

**Was hier rein gehört:** Ein Eintrag pro wesentlicher Änderung, nicht mehr.

**Beispiel (kurz):**

| Version | Datum | Änderung |
|---|---|---|
| 1.1.0 | 2026-10-24 | FSPEC-0002 gestartet, ADR-0005 freigegeben |
| 1.0.0 | 2026-10-20 | Initiale Projektbeschreibung freigegeben |

Faustregel: Endlos-Protokolle, die die Kernaussagen des Dokuments untergraben,
gehören in Freigabezyklus und Git — nicht hierher.
