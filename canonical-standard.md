# Canonical Data Model Standard

## Purpose

The Canonical Data Model establishes a single authoritative request structure used throughout the Platform Engineering ecosystem. All workflow participants MUST derive platform-specific payloads from the Canonical Request Model to ensure consistency, traceability, interoperability, and auditability across all platform integrations. The governance rules require the Orchestrator to construct a canonical request payload prior to policy evaluation.

---

## Architectural Principles

### CDM-1 Single Source of Request Context

The Canonical Request SHALL be the authoritative representation of a provisioning request throughout its lifecycle.

### CDM-2 Data Transformation Restriction

Platform-specific payloads MAY be derived from the Canonical Request but SHALL NOT introduce additional authoritative business attributes.

### CDM-3 Traceability Preservation

The Trace_ID and W3C traceparent SHALL be preserved across all downstream integrations.

### CDM-4 Immutable Request Context

The original Canonical Request SHALL remain immutable after submission. Workflow-generated metadata SHALL be maintained separately.

#### CDM-5 Enterprise Context Enrichment

The Canonical Request SHALL contain both:

- User Submitted Configuration
- Enterprise Resolved Configuration

Enterprise configuration SHALL be resolved by the Red Hat Developer Orchestrator prior to Policy Evaluation.

Resolved configuration SHALL become part of the immutable Canonical Request.

---

## Canonical Request Schema

### Canonical Field Constraints

#### target.environment

Allowed Values:

```yaml
dev
test
stage
prod
```

Values SHALL be lowercase.

#### target.region

Values SHALL conform to the target platform's regional naming standard.

Examples:

```yaml
us-east-1
us-west-2
eu-west-1
```

#### template.version

Version SHALL follow Semantic Versioning.

Example:

```yaml
1.2.0
```

#### resource.type

Values SHALL be selected from the approved resource catalog.

Examples:

```yaml
vm
kubernetes-cluster
database
object-storage
network
```

#### governance.tags

Tags SHALL be key/value pairs.

Example:

```yaml
cost_center: "1234"
application: "payments"
owner: "team-alpha"
```

#### Null Handling

The following fields SHALL NOT be null:

- trace_id
- request_id
- requester.user_id
- template.name
- template.version
- target.environment
- resource.type
- submission.timestamp

Any request containing null values for required fields SHALL be rejected at submission time.
``

```yaml
canonical_request:
  trace_id:
  traceparent:
  request_id:

  requester:
    user_id:
    groups:

  template:
    name:
    version:

  target:
    environment:
    platform:
    region:

  resource:
    type:
    requested_configuration:
    resolved_configuration:

  configuration_context:
    profile_id:
    profile_version:


  governance:
    business_justification:
    tags:

  submission:
    timestamp:
```

---

## Required Fields

| Field | Description | Authoritative Source |
|---------|-------------|----------------------|
| trace_id | Global workflow correlation identifier | RHDH |
| traceparent | Distributed tracing context | RHDH |
| request_id | Unique request identifier | RHDH |
| user_id | Requesting user | Okta |
| groups | User authorization groups | Okta |
| template.name | Executed template | RHDH |
| template.version | Template version | RHDH |
| target.environment | Requested environment | Request |
| resource.type | Requested resource category | Request |
| requested_configuration | Desired configuration | Request |
| tags | Governance and operational tags | Request |
| timestamp | Submission timestamp | RHDH |
| configuration_context.profile_id | Enterprise configuration profile used during request processing | PostgreSQL |
| configuration_context.profile_version | Configuration profile version | PostgreSQL |

---

## Canonical Request Assembly

The Red Hat Developer Orchestrator SHALL construct the Canonical Request using:

- User Request Data
- Identity Context
- Software Template Metadata
- Enterprise Configuration Data

Enterprise configuration data SHALL be resolved prior to Policy Evaluation.

No downstream platform SHALL independently resolve enterprise configuration values.

---

## Derived Payload Rules

The following integrations SHALL derive data from the Canonical Request:

| Consumer | Purpose |
|-----------|----------|
| OPA | Policy Evaluation |
| HCP Terraform | Infrastructure Provisioning |
| EDA | Event Generation |
| AAP | Configuration Activities |
| PostgreSQL | Workflow Metadata |
| S3 | Evidence Package Creation |

---

# Workflow State Machine Standard

## Purpose

The Workflow State Machine defines the authoritative execution lifecycle for all Platform Engineering workflows. Every workflow execution SHALL transition through approved states using the same lifecycle model. This expands upon the governance requirements that define workflow execution records, denial states, approval states, and completion states.

---

## Architectural Principles

### WSM-1 Single Workflow Truth

Workflow state SHALL be authoritatively maintained by the Orchestrator and persisted in PostgreSQL.

### WSM-2 Deterministic State Progression

Workflow transitions SHALL occur only through approved state transitions.

### WSM-3 Auditability

Every state transition SHALL be correlated using the originating Trace_ID.

---

## Authoritative Workflow States

```text
Submitted
Policy_Evaluation
Policy_Denied
Policy_Approved
Provisioning
Provisioning_Failed
Post_Provisioning
Evidence_Assembly
Completed
Cancelled
```

---

## Valid State Transitions

```text
Submitted
    ↓

Policy_Evaluation
    ├──────────────► Policy_Denied
    │                    │
    │                    └──► Cancelled
    │
    └──────────────► Policy_Approved
                           │
                           ▼
                     Provisioning
                           │
         ┌─────────────────┴─────────────────┐
         │                                   │
         ▼                                   ▼

 Post_Provisioning                 Provisioning_Failed
         │                                   │
         ▼                                   ▼

 Evidence_Assembly                  Cancelled
         │
         ▼

 Completed
```

---

## State Ownership

| State | Owning Platform |
|---------|----------------|
| Submitted | RHDH |
| Policy Evaluation | Red Hat Developer Orchestrator |
| Policy Denied | OPA / Orchestrator |
| Policy Approved | OPA / Orchestrator |
| Provisioning | HCP Terraform |
| Provisioning Failed | HCP Terraform |
| Post Provisioning | EDA / AAP |
| Evidence Assembly | Orchestrator |
| Completed | Orchestrator |
| Cancelled | Orchestrator |

---

## State Recording Requirements

Every state transition SHALL record:

```yaml
state_transition:
  trace_id:
  previous_state:
  current_state:
  timestamp:
  actor:
  system:
```

---

# Platform Integration Contract Standard

## Purpose

The Platform Integration Contract Standard defines authoritative interaction boundaries between platforms. Integration behavior SHALL be governed by published contracts rather than implementation-specific decisions.

---

## Architectural Principles

### PIC-1 Contract-First Integration

All platform interactions SHALL be based upon documented contracts.

### PIC-2 Platform Ownership Preservation

Contracts SHALL respect authoritative platform ownership boundaries defined in the Platform Engineering Architecture.

### PIC-3 Canonical Request Consistency

All integrations SHALL consume data derived from the Canonical Request Model.

### PIC-4 Traceability Enforcement

All integration contracts SHALL include Trace_ID propagation requirements. Governance requires Trace_ID propagation across all participating systems.

---

## ICD-001: RHDH → Red Hat Developer Orchestrator

### Purpose

Initiate workflow execution.

### Input

```yaml
canonical_request
```

### Output

```yaml
workflow_id:
trace_id:
submission_status:
```

### Responsibility Matrix

| Platform | Responsibility |
|-----------|---------------|
| RHDH | Request Initiation |
| Red Hat Developer Orchestrator | Workflow Coordination |

---

## ICD-002: Red Hat Developer Orchestrator → OPA

### Purpose

Perform mandatory policy evaluation before any provisioning activity.

### Input

```yaml
canonical_request
```

### Output

```yaml
decision:
  approved|denied

receipt:
  receipt_id:
  policy_version:
  signature:
```

---

## ICD-003: Red Hat Developer Orchestrator → HCP Terraform

### Purpose

Initiate infrastructure provisioning following policy approval.

### Input

```yaml
approved_request:
trace_id:
```

### Output

```yaml
terraform_run_id:
execution_status:
```

---

## ICD-004: Red Hat Developer Orchestrator → EDA

### Purpose

Initiate event-driven post-provisioning automation via webhook.

### Input

```yaml
event:
  provisioning_completed

trace_id:
resource_details:
```

### Output

```yaml
event_id:
event_status:
```

---

## ICD-005: EDA → AAP

### Purpose

Execute approved post-provisioning tasks.

### Input

```yaml
event_id:
trace_id:
job_template:
target_system:
```

### Output

```yaml
job_id:
job_status:
```

---

## ICD-006: Red Hat Developer Orchestrator → S3 Evidence Repository

### Purpose

Persist immutable audit evidence.

### Input

```yaml
trace_id:
canonical_request:
policy_receipt:
deployment_results:
```

### Output

```yaml
evidence_uri:
storage_status:
```

---

## Enterprise Integration Requirements

### EIR-1 Traceability

All integration contracts MUST transmit:

```yaml
trace_id:
traceparent:
```

### EIR-2 Correlation

Every participating platform MUST record the originating Trace_ID in logs, events, execution records, metrics, and evidence repositories.

### EIR-3 Contract Compliance

Platform implementations MUST comply with the published interface contract and SHALL NOT introduce undocumented mandatory fields.

### EIR-4 System of Record Preservation

No platform SHALL assume ownership of data domains outside its documented architectural responsibilities.