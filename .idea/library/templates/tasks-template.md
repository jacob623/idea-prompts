---
offer_id: <OFRXXXXXX>
architecture_id: <arch-id>
offering_type: day1 | day2 | operational
status: Draft
created_date: <UTC date>
---

# Tasks: <offering title>

**Source**: `architectures/<arch-id>/<OFRXXXXXX>.md`
**Standard**: `.idea/library/standards/agile.md`

---

## Epic: Template Implementation
*All offering types.*

### Feature: RHDH Software Template
- feature_id: FTR-001
- source_component: `offer.md ## RHDH Template Definition` + `## Canonical Request Mapping`
- acceptance_criteria: [RHDH template renders with all parameters; canonical request fields are correctly mapped]

#### Story: Implement RHDH Software Template
- story_id: STR-001
- feature_id: FTR-001
- implementation_type: software-template
- source_component: `offer.md ## RHDH Template Definition`
- acceptance_criteria: [Template YAML validates; all offer parameters present; submits canonical request to Orchestrator]
- dependencies: none

**Tasks:**
- [ ] TSK001 | task_type: development | Create RHDH software template YAML — `templates/<offering-name>/template.yaml`
- [ ] TSK002 | task_type: testing | Validate template rendering and parameter mapping — `templates/<offering-name>/template_test.yaml`
- [ ] TSK003 | task_type: documentation | Document template parameters and usage — `templates/<offering-name>/README.md`

---

## Epic: Policy Enforcement
*All offering types.*

### Feature: OPA Provisioning Policy
- feature_id: FTR-002
- source_component: `offer.md ## Policy Gate`
- acceptance_criteria: [Policy approves authorized requests; denies unauthorized requests per stated denial conditions]

#### Story: Implement OPA Policy Bundle
- story_id: STR-002
- feature_id: FTR-002
- implementation_type: policy
- source_component: `offer.md ## Policy Gate`
- acceptance_criteria: [All denial conditions produce `decision: denied`; authorized requests produce `decision: approved` with receipt]
- dependencies: none

**Tasks:**
- [ ] TSK004 | task_type: policy | Write Rego allow/deny rules from Policy Gate authorized roles, environment rules, configuration bounds, and denial conditions — `policies/<policy-name>/policy.rego`
- [ ] TSK005 | task_type: testing | Write Rego unit tests for all approval and denial scenarios — `policies/<policy-name>/policy_test.rego`
- [ ] TSK006 | task_type: documentation | Document policy inputs, outputs, and evaluation logic — `policies/<policy-name>/README.md`

---

## Epic: Provisioning Automation
*Day 1 and Day 2 only. Omit for operational offerings.*

### Feature: Terraform Provisioning Module
- feature_id: FTR-003
- source_component: `offer.md ## Provisioner ### HCP Terraform`
- acceptance_criteria: [Module provisions all resources defined in CMDB Schema; outputs match declared values]

#### Story: Implement Terraform Module
- story_id: STR-003
- feature_id: FTR-003
- implementation_type: implementation-artifact
- source_component: `offer.md ## Provisioner ### HCP Terraform`
- acceptance_criteria: [Terraform plan succeeds; all input variables accepted; outputs declared]
- dependencies: none

**Tasks:**
- [ ] TSK007 | task_type: terraform | Create Terraform module with input variables from Canonical Request Mapping and outputs from Provisioner — `modules/<module-path>/main.tf`, `variables.tf`, `outputs.tf`
- [ ] TSK008 | task_type: testing | Write Terraform test — `modules/<module-path>/tests/`
- [ ] TSK009 | task_type: documentation | Document module inputs, outputs, and usage — `modules/<module-path>/README.md`

---

## Epic: Orchestrator Workflow
*All offering types.*

### Feature: SonataFlow Workflow Definition
- feature_id: FTR-004
- source_component: `offer.md ## Workflow Definition`
- acceptance_criteria: [Workflow executes all steps in order; ICD calls succeed; state transitions per workflow-state-machine.md]

#### Story: Implement SonataFlow Workflow
- story_id: STR-004
- feature_id: FTR-004
- implementation_type: workflow
- source_component: `offer.md ## Workflow Definition`
- acceptance_criteria: [Workflow YAML validates against SonataFlow schema; all states and transitions present]
- dependencies: STR-002 (policy must exist before workflow can invoke ICD-002)

**Tasks:**
- [ ] TSK010 | task_type: workflow | Create SonataFlow YAML from Workflow Definition step table — `workflows/<offering-name>/workflow.sw.yaml`
- [ ] TSK011 | task_type: testing | Write workflow integration test — `workflows/<offering-name>/tests/`
- [ ] TSK012 | task_type: documentation | Document workflow states and ICD calls — `workflows/<offering-name>/README.md`

---

## Epic: Post-Provisioning Automation
*Day 1 and Day 2 only. Omit for operational offerings.*

### Feature: EDA Rulebook
*Include when ICD-005 is applicable per Integration Contracts. Omit otherwise.*
- feature_id: FTR-005
- source_component: `offer.md ## Provisioner ### EDA`
- acceptance_criteria: [Rulebook triggers on correct event; dispatches to correct AAP job template]

#### Story: Implement EDA Rulebook
- story_id: STR-005
- feature_id: FTR-005
- implementation_type: automation-job
- source_component: `offer.md ## Provisioner ### EDA`
- acceptance_criteria: [Rulebook YAML validates; trigger event filter matches ICD-005 event; automation target resolves]
- dependencies: STR-004 (workflow must emit the trigger event)

**Tasks:**
- [ ] TSK013 | task_type: automation | Create EDA rulebook YAML from trigger event and filter conditions — `rulebooks/<rulebook-name>/rulebook.yml`
- [ ] TSK014 | task_type: testing | Write rulebook unit test — `rulebooks/<rulebook-name>/tests/`
- [ ] TSK015 | task_type: documentation | Document rulebook trigger and automation target — `rulebooks/<rulebook-name>/README.md`

### Feature: Ansible Post-Provisioning Playbook
- feature_id: FTR-006
- source_component: `offer.md ## Provisioner ### AAP`
- acceptance_criteria: [Playbook applies all roles; CIS hardening completes; service registered]

#### Story: Implement Ansible Playbook
- story_id: STR-006
- feature_id: FTR-006
- implementation_type: automation-job
- source_component: `offer.md ## Provisioner ### AAP`
- acceptance_criteria: [Playbook runs idempotently; all post-provisioning tasks complete; target inventory resolved from resource_ci.resource_id]
- dependencies: STR-005 (EDA must invoke AAP)

**Tasks:**
- [ ] TSK016 | task_type: automation | Create Ansible playbook with roles from Provisioner AAP section — `playbooks/<job-template-name>/playbook.yml`
- [ ] TSK017 | task_type: testing | Write playbook molecule test — `playbooks/<job-template-name>/molecule/`
- [ ] TSK018 | task_type: documentation | Document playbook roles and post-provisioning tasks — `playbooks/<job-template-name>/README.md`

---

## Epic: Database Schema
*All offering types.*

### Feature: Flyway Schema Migration
- feature_id: FTR-007
- source_component: `offer.md ## CMDB Schema`
- acceptance_criteria: [All tables defined in CMDB Schema are created; migration is idempotent; Flyway checksum validates]

#### Story: Implement Flyway Migrations
- story_id: STR-007
- feature_id: FTR-007
- implementation_type: database-schema
- source_component: `offer.md ## CMDB Schema`
- acceptance_criteria: [Flyway migrate succeeds on clean database; all resource_ci and provision_event columns present]
- dependencies: none

**Tasks:**
- [ ] TSK019 | task_type: database | Create Flyway versioned migration for each table in CMDB Schema — `db/migrations/V{n}__<offering-name>_schema.sql`
- [ ] TSK020 | task_type: testing | Write migration integration test — `db/migrations/tests/`
- [ ] TSK021 | task_type: documentation | Document schema tables and fields — `db/migrations/README.md`

---

## Dependencies & Execution Order

| Story | Depends on | Rationale |
|---|---|---|
| STR-001 RHDH Template | — | No dependencies |
| STR-002 OPA Policy | — | No dependencies |
| STR-003 Terraform Module | — | No dependencies |
| STR-004 SonataFlow Workflow | STR-002 | Policy must exist before workflow invokes ICD-002 |
| STR-005 EDA Rulebook | STR-004 | Workflow must emit trigger event |
| STR-006 Ansible Playbook | STR-005 | EDA must invoke AAP |
| STR-007 Flyway Migration | — | No dependencies |

STR-001, STR-002, STR-003, STR-007 can proceed in parallel.

## Traceability
- offer_id: <OFRXXXXXX>
- architecture_id: <arch-id>
