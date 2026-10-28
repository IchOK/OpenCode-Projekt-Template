---
id: PROJECT-XXXX
title: "Project title"
version: 0.1.0
template_version: 0.1.0
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

# Project Description

**Positioning:** This document conveys project context and development goals. It does not replace approved requirements, functional specifications, or ADRs. Statements about concrete components reflect the current planning state, not immutable rules.

## 1. Purpose and Vision

Describe the problem the project solves, the rough system concept (major subsystems and their relationship), the intended operating environment, and the chosen development approach (e.g. staged proofs of concept instead of one-shot delivery). State how later extension is intended.

## 2. Goals and Success Criteria at Project Level

List the goals as measurable statements (target values, tolerances, operating conditions). Mark anything not yet formalized as such. Note explicitly that goals at this level are **not yet formally approved individual requirements**; measurement methods, tolerances and acceptance criteria are defined in requirements and functional specifications.

## 3. Planned Use Cases

List the intended scenarios as short numbered use cases: who operates the system, under which conditions, and which system behavior defines success for each scenario.

## 4. System Overview

Describe the major subsystems in subsections, one per subsystem where useful:

- **Hardware:** selected components and variants, operating locations, platform choices and any planned platform-evaluation path.
- **Software:** language/runtime choices per component; state the decision-driving rationale and reference the corresponding ADR.
- **Connectivity:** communication paths and protocols between subsystems, including data that must be transported and what must be verified about interoperability.
- **Backend/operations:** role of any central planning, monitoring or logging component; per-component operational limits (e.g. one active task at a time, configurable parameters).

Each unconfirmed selection is marked as to-be-confirmed rather than presented as fixed.

## 5. Operation, Safety and Control

Describe the operating concept: what human release is required before operation, how states are reported, defined stop or fallback behavior on communication, positioning or actuator faults, authorization handling for updates/deployments, and which measures are proven versus which are development approaches not yet proven.

## 6. Project Boundaries and Non-Goals

List explicitly what the initial scope excludes — typically: full autonomy or operation in unknown environments, guaranteed safety features not yet proven, regulatory approval or product maturity, and permanent commitment to specific tools, models or frameworks. This section prevents goals from being read as silent commitments.

## 7. Planned Milestones

| Phase | Content | Evidence (preliminary) |
|---|---|---|
| M1 | <milestone goal> | <observable proof> |
| M2 | <milestone goal> | <observable proof> |

State that sequence and acceptance criteria are concretized through approved specifications and that the table is a roadmap, not an implementation assignment.

## 8. Open Decisions and Risks

List open threads as bullets, grouped where helpful: unresolved component selection, technical feasibility questions, operational/safety risks, and external conditions. Point to the loose-end register (todo-register) for items that arise along the way. State that new binding decisions follow the ADR/requirement/specification cycles — not this document.

## 9. Development and Documentation Process

- Version control and hosting location.
- Links to the process rule sets: [`../process/document-lifecycle.md`](../process/document-lifecycle.md), [`../process/multi-agent-orchestration.md`](../process/multi-agent-orchestration.md).
- Binding agent instructions: [`../../AGENTS.md`](../../AGENTS.md).
- The workflow chain: see the active project's project description, §9: [`../../project/project.md`](../../project/project.md).

> **Template notice:** This document is the template for creating a new project; it does not describe an active project. The referenced workflow chain belongs to the active project.
- Concurrency note: the coordinator may use at most <n> parallel workers under the conditions defined in the orchestration process document.
- Model and tool assignments are interchangeable runtime configuration; actual models used are logged per task where possible.

## 10. Change History

| Version | Date | Change |
|---|---|---|
| 0.1.0 | YYYY-MM-DD | <initial version> |
