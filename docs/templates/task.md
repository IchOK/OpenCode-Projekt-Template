---
id: TASK-XXXX   # Task-ID nach Nummerungskonvention in docs/templates/tasks-for-fspec.md
title: "Task title"
version: 0.1.0
template_version: 0.1.0
created_at: YYYY-MM-DD
modified_at: YYYY-MM-DD
author: ""
assigned_to: ""
status: Open   # Task status only; FSPEC implementation status is tracked via references.functional_spec
priority: Medium
references:
  functional_spec:
    id: FSPEC-XXXX
    version: 1.0.0
    acceptance_criteria: []
  tasks: []
depends_on: []
blocked_by: []
files_scope: []
repository: null   # Code-Repo des Tasks, z. B. esp32-mesh-framework-gateway; null = Main-only (ADR-0003)
result_commit: null   # bei Integration als repo@sha führen
completed_at: null
---

## Description
Unambiguous task derived from approved FSPEC.

## Work steps
- [ ] Prepare implementation
- [ ] Implement
- [ ] Add tests
- [ ] Run tests
- [ ] Update documentation

## Completion criteria
- Integrated and verified changes
- Relevant tests pass
- FSPEC acceptance criteria met

## Result
- Changes and files:
- Commits:
- Tests and results:
- Technical decisions:
- Known limitations:
- Execution log reference:
