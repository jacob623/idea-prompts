# Workflow State Machine Standard

## Purpose

The Workflow State Machine Standard defines the authoritative execution lifecycle for all Platform Engineering workflows. Every workflow execution SHALL transition through approved states using a common lifecycle model to ensure consistency, traceability, auditability, and deterministic execution behavior. This standard complements the Platform Engineering Workflow Governance Rules by defining the authoritative workflow execution states and permissible state transitions.

---

## Scope

This standard applies to all workflow executions initiated through Red Hat Developer Hub (RHDH) and coordinated by the Red Hat Developer Orchestrator. All participating platforms MUST adhere to the workflow states and transition requirements defined in this standard.

---

## Architectural Principles

### WSM-1 Single Source of Workflow State

The Red Hat Developer Orchestrator SHALL be the authoritative owner of workflow execution state, and PostgreSQL SHALL be the authoritative repository for workflow state persistence and execution history. 

### WSM-2 Deterministic Execution

Workflow executions SHALL progress only through approved state transitions defined within this standard.

### WSM-3 Traceability

Every workflow state transition SHALL be correlated using the originating Trace_ID.

### WSM-4 Fail-Closed Operation

Workflow execution SHALL immediately halt whenever a mandatory control or governance validation fails.

---

## Workflow States

### Submitted

The workflow request has been accepted by RHDH and assigned a Trace_ID.

Entry Criteria:

- User successfully authenticated through Okta
- Request parameters validated
- Trace_ID generated
- Request submitted to the Orchestrator

Exit Criteria:

- Execution record created in PostgreSQL
- Workflow enters Policy_Evaluation

---

#### Configuration_Resolution

The Orchestrator resolves enterprise configuration data required for execution.

Entry Criteria:

- Workflow submitted
- Execution record created

Required Actions:

- Load Configuration Profile
- Resolve Enterprise Configuration
- Determine Automation Platform
- Determine Approved Module
- Construct Resolved Configuration

Exit Criteria:

- Policy_Evaluation
- Cancelled

---

### Policy_Evaluation

The Orchestrator is preparing and submitting the canonical request to OPA for governance validation.

Entry Criteria:

- Execution record exists
- Canonical request payload generated

Exit Criteria:

- Policy_Approved
- Policy_Denied

---

### Policy_Denied

OPA has denied the request.

Entry Criteria:

- OPA evaluation returns Denied

Required Actions:

- Record denial decision
- Write denial event to Splunk
- Update execution state
- Halt workflow execution

Exit Criteria:

- Cancelled
- Policy_Evaluation (administrative override and resume)

---

### Policy_Approved

OPA has approved the request.

Entry Criteria:

- OPA evaluation returns Approved
- Signed policy receipt received

Required Actions:

- Store approval receipt
- Update execution metadata

Exit Criteria:

- Provisioning

---

### Provisioning

Infrastructure resources are being provisioned using HCP Terraform.

Entry Criteria:

- Approved OPA decision recorded

Required Actions:

- Initiate Terraform run
- Persist Terraform Run ID
- Monitor execution status

Exit Criteria:

- Post_Provisioning
- Provisioning_Failed

---

### Provisioning_Failed

Infrastructure provisioning has failed.

Entry Criteria:

- Terraform execution failure

Required Actions:

- Record failure event
- Persist execution details
- Log associated Trace_ID

Exit Criteria:

- Cancelled

---

### Post_Provisioning

Event-Driven Ansible (EDA) and Ansible Automation Platform (AAP) are executing post-provisioning automation.

Entry Criteria:

- Successful infrastructure provisioning

Required Activities:

- Event processing
- Operating system hardening
- Agent installation
- Application onboarding
- Service registration
- Organizational controls

Exit Criteria:

- Evidence_Assembly

---

### Evidence_Assembly

The Orchestrator is compiling the deployment evidence package.

Entry Criteria:

- Successful completion of post-provisioning activities

Required Actions:

- Collect canonical request
- Collect policy receipt
- Collect deployment results
- Assemble audit package
- Store evidence in S3

Exit Criteria:

- Completed

---

### Completed

Workflow execution has successfully completed.

Entry Criteria:

- Evidence package successfully stored

Required Actions:

- Record completion timestamp
- Record duration
- Update execution state

Terminal State:

- Yes

---

### Cancelled

Workflow execution has been terminated and will not continue.

Entry Criteria:

- Administrative cancellation
- Policy denial without approval
- Unrecoverable execution failure

Terminal State:

- Yes

---

## State Transition Model

```text
Submitted
    │
    ▼
Configuration_Resolution
    │
    ▼
Policy_Evaluation
    ├──────────────► Policy_Denied
    │                    │
    │                    ├────► Cancelled
    │                    │
    │                    └────► Policy_Evaluation
    │                            (Administrative Resume)
    │
    ▼
Policy_Approved
    │
    ▼
Provisioning
    ├──────────────► Provisioning_Failed
    │                    │
    │                    ▼
    │               Cancelled
    │
    ▼
Post_Provisioning
    │
    ▼
Evidence_Assembly
    │
    ▼
Completed
```

---

## State Ownership Matrix

| Workflow State | Authoritative Owner |
|----------------|---------------------|
| Submitted | RHDH |
| Policy_Evaluation | Red Hat Developer Orchestrator |
| Policy_Denied | OPA / Orchestrator |
| Policy_Approved | OPA / Orchestrator |
| Provisioning | HCP Terraform |
| Provisioning_Failed | HCP Terraform |
| Post_Provisioning | EDA / AAP |
| Evidence_Assembly | Red Hat Developer Orchestrator |
| Completed | Red Hat Developer Orchestrator |
| Cancelled | Red Hat Developer Orchestrator |
| Configuration_Resolution | Red Hat Developer Orchestrator |

---

## Mandatory State Metadata

Every state transition SHALL record:

```yaml
state_transition:
  trace_id:
  workflow_id:
  previous_state:
  current_state:
  transition_timestamp:
  initiating_system:
  initiating_actor:
  status:
```

---

## State Persistence Requirements

The Orchestrator SHALL persist the following information for each state transition within PostgreSQL:

```yaml
workflow_execution_record:
  workflow_id:
  trace_id:
  state:
  previous_state:
  timestamp:
  actor:
  system:
  message:
```

---

## Audit Requirements

### WSM-A1 Trace Correlation

All state transitions MUST remain correlated through the originating Trace_ID.

### WSM-A2 Historical Retention

All workflow state transitions MUST be retained as part of execution history.

### WSM-A3 Reconstruction Support

Authorized auditors MUST be able to reconstruct the complete workflow lifecycle using the Trace_ID and workflow execution records.

### WSM-A4 Terminal State Enforcement

Only the `Completed` and `Cancelled` states SHALL be considered terminal workflow states.

---

## Compliance Requirements

Workflow implementations SHALL NOT:

- Skip defined workflow states.
- Introduce undocumented workflow states.
- Bypass state transition recording.
- Modify historical state transition records.
- Progress beyond Policy_Evaluation when OPA returns a denied decision unless an authorized administrative override has been approved and recorded.

All participating platforms MUST propagate the Trace_ID throughout workflow execution to maintain end-to-end traceability. 