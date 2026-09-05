# Platform Engineering Workflow Governance Rules

The Platform Engineering Workflow Governance Rules establish the mandatory governance, security, compliance, traceability, automation, and audit requirements that all Platform Engineering workflows must follow. These rules ensure that every workflow is executed in a consistent, secure, and fully traceable manner from request initiation through final deployment, evidence retention, and audit review.

Compliance with these Workflow Governance Rules is mandatory for all systems, workflows, templates, automation components, and integrations participating in the provisioning and delivery lifecycle.

## 1. Identity, Authentication, and Request Origination

### R-1.1 Authentication Requirement
All users **MUST** authenticate to Red Hat Developer Hub (RHDH) through Okta OIDC before initiating any Software Template execution.

### R-1.2 Role Inheritance
User authorization **MUST** be derived from Okta group membership and mapped to approved platform roles.

### R-1.3 Approved Template Requirement
Only approved and onboarded Software Templates **MAY** be executed through RHDH.

### R-1.4 Required Parameter Validation
All required template input parameters **MUST** be provided and validated prior to request submission.

### R-1.5 Traceability Identifier Generation
RHDH **MUST** generate a cryptographically secure and globally unique `Trace_ID` for every template execution request.

### R-1.6 Trace Context Requirement
The Enterprise Scaffolder Extension **MUST** generate a W3C-compliant `traceparent` context associated with the `Trace_ID`.

### R-1.7 Server-Side Generation
`Trace_ID` and `traceparent` values **MUST** be generated server-side and **MUST NOT** be supplied by end users.

### R-1.8 Request Propagation
RHDH **MUST** forward the complete request payload and generated `Trace_ID` to the Red Hat Developer Orchestrator.

---

## 2. Execution Metadata Management

### R-2.1 Initial Record Creation
The Orchestrator **MUST** create an execution record in PostgreSQL before any provisioning activities begin.

### R-2.2 Required Metadata Fields
The execution record **MUST** contain the following fields:

- Start Timestamp
- Trace_ID
- User ID
- Template Name
- Input Parameters
- Orchestrator Workflow ID
- S3 Evidence URI
- State = `In Progress`
- Policy Decision = `In Progress`
- Configuration Profile ID
- Configuration Profile Version

### R-2.3 Trace Correlation
All workflow records **MUST** remain correlated through the generated `Trace_ID`.

---

## 3. Policy Enforcement Gate (OPA)

### Policy Decision Determinism Standard

#### PD-1 Policy Version Resolution

OPA SHALL evaluate requests using the active policy bundle version identified at the time the workflow enters the Policy_Evaluation state.

The evaluated policy version SHALL be recorded in:
- Policy receipt
- PostgreSQL workflow metadata
- Audit evidence package

#### PD-2 Evaluation Outcome

OPA SHALL return exactly one of the following outcomes:
- Approved
- Denied

No additional decision states are permitted.

#### PD-3 Rule Conflict Resolution

When multiple policies evaluate the same request:

1. Explicit Deny SHALL take precedence over Approve.
2. More restrictive controls SHALL take precedence over less restrictive controls.
3. If policy applicability cannot be determined, the request SHALL be Denied.
4. Ambiguous evaluations SHALL result in Denied.

#### PD-4 Waiver Processing

Approved policy waivers SHALL be evaluated after mandatory controls and before optional controls.

Mandatory controls SHALL NOT be bypassed through waivers unless explicitly approved through the administrative override process.

#### PD-5 Deterministic Evaluation Inputs

Policy decisions SHALL be derived exclusively from:
- Canonical Request
- Approved Policy Bundle
- Approved Waiver Records

OPA SHALL NOT consume undocumented data sources during evaluation.

#### PD-6 Decision Reproducibility

The same Canonical Request, Policy Bundle Version, and Waiver Set SHALL produce the same policy decision.

### R-3.0 Enterprise Configuration Resolution

Prior to Policy Evaluation, the Orchestrator MUST resolve all required enterprise configuration data.

Configuration resolution MUST retrieve:

- Resource Configuration
- Environment Configuration
- Platform Configuration
- Automation Platform Selection
- Approved Module Selection

Resolved values MUST be incorporated into the Canonical Request.

Provisioning SHALL NOT proceed if required configuration attributes cannot be resolved.

### R-3.1 Mandatory Policy Evaluation
Every workflow execution **MUST** undergo a pre-flight Open Policy Agent (OPA) evaluation prior to any provisioning activity.

#### R-3.2 Canonical Payload Generation

The Orchestrator MUST construct a Canonical Request using:

- User Request Data
- Identity Context
- Template Metadata
- Enterprise Configuration Data

Enterprise configuration values **MUST** be resolved before Policy Evaluation begins.

### R-3.3 REST-Based Evaluation
The Orchestrator **MUST** submit the policy payload to OPA through a REST API request.

### R-3.4 Policy Scope
OPA **MUST** evaluate, at minimum:

- Resource sizing
- Target environment
- Naming conventions
- Security groups
- Mandatory tags
- Additional active governance policies

### R-3.5 Fail-Closed Enforcement
Infrastructure provisioning **MUST NOT** proceed when an OPA evaluation results in a denied decision.

### R-3.6 Violation Logging
All policy violations **MUST** be logged to Splunk and correlated with the originating `Trace_ID`.

### R-3.7 Workflow Suspension on Denial
The workflow **MUST** halt immediately upon a denied policy decision.

### R-3.8 Administrative Override Authority
Only authorized Platform Administrators **MAY** apply approved parameter overrides or policy waivers.

### R-3.9 Manual Resume Requirement

#### R-3.9A Administrative Override Procedure

Administrative override requests SHALL contain:

- Trace_ID
- Request ID
- Justification
- Override Scope
- Approver Identity
- Approval Timestamp

#### R-3.9B Approved Override Requirements
Overrides SHALL be:

- Explicitly documented
- Traceable
- Time bounded
- Included within the evidence package

#### R-3.9C Override Audit Record

The Orchestrator SHALL persist:

```yaml
override_record:
  trace_id:
  approver:
  justification:
  approved_timestamp:
  expiration_timestamp:
  affected_controls:
```

#### R-3.9D Resume Authorization

Only users assigned the Platform Administrator role through Okta MAY resume denied workflows.
Denied workflows **MAY** resume only through explicit administrator action.

### R-3.10 Denied State Recording
Upon denial, the Orchestrator **MUST** update PostgreSQL with:

```text
State = Error
Policy Decision = Denied
```

### R-3.11 Signed Approval Receipt
OPA **MUST** issue a signed policy decision receipt for approved requests.

### R-3.12 Evidence Preservation
The signed OPA approval receipt **MUST** be written to the designated Evidence URI.

### R-3.13 Approval Recording
Upon approval, the Orchestrator **MUST** update PostgreSQL with:

```text
Policy Decision = Approved
```

---

## 4. Infrastructure Provisioning Controls

### R-4.1 Provisioning Prerequisite
Infrastructure provisioning **MUST** begin only after an approved OPA decision has been recorded.

### R-4.2 Terraform Initiation
The Orchestrator **MUST** initiate an HCP Terraform workspace run and pass the associated `Trace_ID`.
HCP Terraform workspace runs **MUST** be initiated by the Orchestrator.

### R-4.3 Terraform Trace_ID
The Orchestrator **MUST** pass the associated `Trace_ID` when initiating HCP Terraform workspace runs.

### R-4.4 Run Tracking
The Orchestration **MUST** record the Terraform Run ID within PostgreSQL.

### R-4.5 Declarative Provisioning Requirement
Infrastructure resources **MUST** be provisioned using approved declarative Terraform configurations when a supported module exists.

#### R-4.5A Approved Module Definition

An approved module SHALL:

- Be stored in the approved Terraform registry
- Have an assigned owner
- Have a semantic version
- Pass security validation
- Pass policy validation

#### R-4.5B Module Selection

Provisioning workflows SHALL use the latest approved module version unless a specific version is explicitly requested.

#### R-4.5C Unsupported Resources

If no approved Terraform module exists:

1. Request SHALL enter manual review.
2. Provisioning SHALL NOT proceed automatically.
3. Approval SHALL be recorded.

### R-4.6 Resource Tagging
Provisioned resources **MUST** be tagged with the `Trace_ID` where platform capabilities permit.

### R-4.7 Workspace Traceability
Terraform workspace configuration **MUST** include the `Trace_ID` to support searchability and cross-workspace correlation.

### R-4.8 State File Traceability
Terraform state outputs **MUST** contain the originating `Trace_ID`.

### R-4.9 Drift Investigation Support
Terraform state metadata **MUST** support identification of the originating request during drift investigations.

---

## 5. Observability and Monitoring

### R-5.1 Orchestrator Metrics Collection
Orchestrator run metrics **MUST** be published to Dynatrace using the approved native integration.

### R-5.2 Terraform Metrics Collection
Terraform run metrics **MUST** be published to Dynatrace using the approved native integration.

### R-5.3 AAP Metrics Collection
AAP execution metrics **MUST** be exported to Dynatrace through the approved OpenTelemetry callback integration.

### R-5.4 Continuous Log Streaming
Execution logs **MUST** be continuously streamed to Splunk throughout workflow execution.

### R-5.5 Correlation Standard
All logs **MUST** include the originating `Trace_ID`.

---

## 6. Event-Driven Post-Provisioning Automation

### R-6.1 Provisioning Completion Event
Event-Driven Ansible (EDA) **MUST** be initiaited by a webhook event by the Orchestrator.

### R-6.2 Event Processing
Event-Driven Ansible (EDA) **MUST** consume and evaluate the event against approved rulebooks.

### R-6.3 Job Template Invocation
EDA **MUST** invoke the designated Ansible Automation Platform (AAP) job template when matching criteria are satisfied.

### R-6.4 Post-Provisioning Activities
AAP **MUST** execute required post-provisioning tasks, including where applicable:

- Operating system hardening
- Agent installation
- Application onboarding
- Service registration
- Additional organizational controls

#### R-6.5 Post-Provisioning Decision Matrix

Applicability SHALL be determined using resource type and target environment.

Example:

| Activity | VM | Kubernetes | Database |
|-----------|----|------------|----------|
| OS Hardening | Required | Not Applicable | Not Applicable |
| Agent Installation | Required | Required | Required |
| Service Registration | Required | Required | Required |
| Application Onboarding | Conditional | Conditional | Conditional |

#### R-6.6 Automation Catalog

AAP job templates SHALL be mapped to resource types through a centrally managed automation catalog.

#### R-6.7 Deterministic Execution

Given the same:
- Resource Type
- Environment
- Automation Catalog Version

the same set of post-provisioning jobs SHALL be selected.

---

## 7. Evidence Collection and Retention

### R-7.1 Evidence Compilation
The Orchestrator **MUST** compile a complete deployment evidence package upon workflow completion.

### R-7.2 Mandatory Evidence Contents
The evidence package **MUST** contain:

- Original cononical request
- Signed OPA evaluation receipt

### R-7.3 Immutable Storage Requirement
Evidence artifacts **MUST** be stored within an immutable S3 repository.

### R-7.4 Evidence Storage Path
Evidence artifacts **MUST** be stored using the following path structure:

```text
/audit-evidence/{Year}/{Month}/{Trace_ID}.json
```

### R-7.5 Integrity Preservation
Evidence artifacts **MUST NOT** be modified after creation.

---

## 8. Audit Record Lifecycle Management

### R-8.1 Workflow Completion Recording
Upon successful completion, PostgreSQL **MUST** be updated with:

```text
State = Complete
Duration
Closed Timestamp
```

### R-8.2 Audit Correlation
All audit records **MUST** remain linked through the originating `Trace_ID`.

### R-8.3 Centralized Log Retention
Final execution logs from the following systems **MUST** be indexed in Splunk using the associated `Trace_ID`:

- RHDH
- Red Hat Developer Orchestrator
- OPA
- HCP Terraform
- EDA
- AAP

---

## 9. Metrics and Reporting

### R-9.1 Operational Metrics Collection
Dynatrace **MUST** collect workflow execution metrics across the provisioning lifecycle.

### R-9.2 Time-to-Delivery Reporting
Dynatrace dashboards **MUST** calculate and display Software Template Time-To-Delivery (TTD).

### R-9.3 Execution Reporting
Dynatrace **MUST** maintain metrics for workflow execution counts, durations, and outcomes.

---

## 10. Traceability and Compliance

### R-10.1 End-to-End Traceability
Every execution request **MUST** be traceable from initiation through evidence archival using a single `Trace_ID`.

### R-10.2 Cross-System Correlation
The `Trace_ID` **MUST** be propagated across:

- RHDH
- Red Hat Developer Orchestrator
- OPA
- HCP Terraform
- EDA
- AAP
- PostgreSQL
- Splunk
- Dynatrace
- S3 Evidence Storage

### R-10.3 Audit Reconstruction
Authorized auditors **MUST** be able to reconstruct the complete workflow lifecycle using only the `Trace_ID`.

### R-10.4 Control Bypass Prohibition
No workflow execution **MAY** bypass authentication, policy evaluation, logging, traceability, monitoring, or evidence retention controls defined in this policy.

### R-10.5 Compliance Enforcement
All platform components participating in the provisioning workflow **MUST** enforce the controls defined in this policy as a condition of operation.

### R-10.6 Compliance Injection
If a workflow is proposed that omits any required process step, control, validation, audit function, traceability element, security measure, or compliance requirement, those missing requirements **MUST** be automatically injected into the workflow definition.