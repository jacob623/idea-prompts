---
codify_id: <CDYXXXXXX>
offer_id: <OFRXXXXXX>
architecture_id: <arch-id>
offering_type: day1 | day2 | operational
status: Draft
completed_at: <UTC timestamp>
---

# Implementation Report: <offering title>

**Source**: `architectures/<arch-id>/<OFRXXXXXX>-pkg/tasks.md`
**Standard**: `.idea/library/standards/coding.md`

## Implementation Summary
- **offering_type**: [day1 | day2 | operational]
- **Epics completed**: [count]
- **Artifacts created**: [count]
- **Unresolved items**: [count — 0 if all tasks completed]

---

## Epic: Template Implementation

### Artifact: RHDH Software Template
- **artifact_path**: `templates/<offering-name>/template.yaml`
- **task_type**: software-template
- **verification_status**: pass | fail | skipped
- **notes**: [any verification output or reason for skip]

---

## Epic: Policy Enforcement

### Artifact: OPA Policy
- **artifact_path**: `policies/<policy-name>/policy.rego`
- **task_type**: policy
- **verification_status**: pass | fail | skipped
- **notes**: [rego test output summary]

---

## Epic: Provisioning Automation
*Day 1 and Day 2 only. Omit for operational offerings.*

### Artifact: Terraform Module
- **artifact_path**: `modules/<module-path>/main.tf`
- **task_type**: terraform
- **verification_status**: pass | fail | skipped
- **notes**: [terraform validate output]

---

## Epic: Orchestrator Workflow

### Artifact: SonataFlow Workflow
- **artifact_path**: `workflows/<offering-name>/workflow.sw.yaml`
- **task_type**: workflow
- **verification_status**: pass | fail | skipped
- **notes**: [schema validation output]

---

## Epic: Post-Provisioning Automation
*Day 1 and Day 2 only. Omit for operational offerings.*

### Artifact: EDA Rulebook
*Omit if ICD-005 is not applicable.*
- **artifact_path**: `rulebooks/<rulebook-name>/rulebook.yml`
- **task_type**: automation-job
- **verification_status**: pass | fail | skipped
- **notes**: [validation output]

### Artifact: Ansible Playbook
- **artifact_path**: `playbooks/<job-template-name>/playbook.yml`
- **task_type**: automation-job
- **verification_status**: pass | fail | skipped
- **notes**: [molecule test output summary]

---

## Epic: Database Schema

### Artifact: Flyway Migration
- **artifact_path**: `db/migrations/V{n}__<offering-name>_schema.sql`
- **task_type**: database-schema
- **verification_status**: pass | fail | skipped
- **notes**: [flyway validate output]

---

## Unresolved Items
[List any tasks that could not be completed, with the reason. If none, state "None."]

| Task ID | Description | Reason |
|---|---|---|
| [TSKNNN] | [task description] | [why it could not be completed] |

## Traceability
- tasks_id: <TSKXXXXXX>
- offer_id: <OFRXXXXXX>
- architecture_id: <arch-id>
