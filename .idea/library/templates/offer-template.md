---
offer_id: <OFRXXXXXX>
title: "<Self-Service Offering Name>"
status: Draft
owner: Platform Engineering
architecture_id: <arch-id>
activity_id: <ACTNNN>
resource_type: <resource-type>
offering_type: day1 | day2 | operational
created_date: <UTC date>
---

# <title>

## Overview
[What this offering provisions, who initiates it, and what outcome it delivers.]

## Target Resource
*Day 2 and operational offerings only. Day 1 offerings leave this section blank.*
[The existing resource_ci this offering operates against.
- **resource_type**: [Resource type of the target, e.g. aws-ec2-instance]
- **Selection method**: [How the user identifies the target in the RHDH template — by CI ID, application + resource name, etc.]
- **Validation**: [How the offering confirms the target CI has status: active before proceeding]]

## Source Traceability
[Reference Architecture: `architectures/<arch-id>/ref.md`]
[Activity: `architectures/<arch-id>/<activity-artifact>`]
[ADR: `architectures/<arch-id>/<adr-artifact>`]

## Compliance Alignment
[Compliance posture was evaluated in the ADR. Do not re-list controls here.
- **ADR**: `architectures/<arch-id>/<adr-artifact>`
- **OPA policy**: [Policy name that enforces runtime governance for this offering]
- **CIS hardening**: [AAP job template that applies CIS benchmark hardening post-provisioning]]

## RHDH Template Definition
[Template name and the input parameters the developer fills in. Each parameter
maps to a canonical request field in the section below.]

## Canonical Request Mapping
[Table mapping every RHDH template parameter to its canonical request field per
`.idea/library/standards/canonical-request.md`.]

| RHDH Parameter | Canonical Request Field | Required |
|---|---|---|
| [parameter] | [canonical_request.field] | Yes / No |

## Configuration Profile
[Which PostgreSQL configuration profile resolves enterprise configuration for this
offering. Resolution order per `.idea/library/standards/configuration-resolution.md`.]

## Policy Gate
[Per `.idea/library/standards/opa-policy.md`. Complete all fields.]
- **Policy name**: [e.g. platform-engineering-provisioning-v1]
- **Invoked by**: ICD-002 (Orchestrator → OPA) before any provisioning begins
- **Authorized roles**: [Okta roles or groups permitted to invoke this offering, e.g. platform-engineer, platform-admin]
- **Environment rules**: [Any environment-specific restrictions, e.g. prod requires role=platform-admin]
- **Configuration bounds**: [Parameter constraints OPA validates, e.g. instance_type ∈ {t3.medium, t3.large}; region ∈ {us-east-1, eu-west-1}]
- **Denial conditions**: [Explicit conditions that trigger a denied decision, e.g. environment=prod AND role≠platform-admin; requested_region not in approved list]

## Workflow Definition
**Format**: SonataFlow YAML (Red Hat Developer Orchestrator)
[Orchestrator workflow steps with state transitions per
`.idea/library/standards/workflow-state-machine.md`. Each step names the platform
it calls and the ICD it uses.]

| Step | Action | Platform | ICD | On Failure |
|---|---|---|---|---|
| 1 | [action] | [platform] | [ICD-NNN] | [behaviour] |

## Provisioner

### HCP Terraform
*Day 1 and Day 2 offerings only. Mark N/A for operational offerings.*
- **Module path**: [e.g. terraform-aws-platform/sqs-queue]
- **Input variables**: [Table mapping canonical request fields to Terraform variable names]
  | Canonical request field | Terraform variable | Type |
  |---|---|---|
  | [field] | [var_name] | string / number / bool |
- **Outputs**: [Resources created and their exported identifiers, e.g. queue_arn, queue_url]

### EDA
*Include when ICD-005 is applicable. Mark N/A otherwise.*
- **Rulebook name**: [e.g. platform-provisioning-completed]
- **Trigger event**: [e.g. provisioning_completed from ICD-005]
- **Filter conditions**: [Conditions the rulebook evaluates before dispatching, e.g. event.resource_type == "aws-sqs-queue"]
- **Automation target**: [AAP job template invoked by this rulebook]

### AAP
- **Job template name**: [e.g. platform-post-provision-sqs]
- **CIS hardening template**: [Job template that applies CIS benchmark hardening, per Compliance Alignment]
- **Playbook roles**: [Ansible roles executed, e.g. aws_cis_sqs, platform_service_registration]
- **Post-provisioning tasks**: [Summary of what the playbook configures after provisioning, e.g. apply security group, register with service catalog, notify monitoring]

## CMDB Schema

### `resource_ci`
[One entry per distinct resource type this offering provisions.]

| Field | Type | Description |
|---|---|---|
| ci_id | UUID | Platform-assigned identifier |
| application_ci_id | UUID | FK → application_ci — parent application |
| resource_type | string | e.g. aws-sqs-queue, aws-lambda-function |
| resource_name | string | Platform-assigned or derived name |
| resource_id | string | Platform ARN or equivalent |
| data_center | string | e.g. us-east-1 |
| availability_zone | string | e.g. us-east-1a |
| environment | enum | dev \| qa \| prod |
| offering_id | string | OFRXXXXXX |
| offering_version | string | Offering version at time of provisioning |
| offering_input | JSONB | Full parameter set submitted to the RHDH template |
| trace_id | string | RHDH-generated trace ID for this request |
| workflow_id | string | Orchestrator workflow execution ID |
| provisioned_at | timestamp | |
| provisioned_by | string | User who submitted the request |
| status | enum | active \| decommissioned |

### `provision_event`
[Event record for every lifecycle action taken against a resource CI. Day 1 and Day 2 offerings only.]

| Field | Type | Description |
|---|---|---|
| event_id | UUID | Surrogate PK — stable identifier for this event |
| ci_id | UUID | FK → resource_ci.ci_id — stable anchor across lifecycle |
| target_resource_ci_id | UUID | FK → resource_ci.ci_id — existing CI affected by a Day 2 action (nullable; null for Day 1) |
| trace_id | string | The request that triggered this event |
| action | enum | created \| modified \| replaced \| decommissioned |
| change_detail | JSONB | Nullable — `{ field, old_value, new_value }` for modified and replaced events |
| evidence_uri | string | S3 path to the evidence package for this event |
| actioned_at | timestamp | |
| actioned_by | string | User who submitted the request |

> `replaced` — resource destroyed and re-provisioned under the same logical identity
> (cattle replacement). `ci_id` remains constant; `trace_id` changes with each request.
> `target_resource_ci_id` — null for Day 1 `created` events; populated for Day 2 actions
> where the affected CI differs from the CI being created (e.g., a firewall rule CI
> created to protect an existing SQS queue CI).

### `resource_relationship`
[CI-to-CI associations created by Day 2 offerings. Day 2 only.]

| Field | Type | Description |
|---|---|---|
| relationship_id | UUID | Surrogate PK |
| source_ci_id | UUID | FK → resource_ci.ci_id (e.g. the firewall rule CI) |
| target_ci_id | UUID | FK → resource_ci.ci_id (e.g. the SQS queue CI) |
| relationship_type | enum | protects \| depends_on \| routes_to \| replicates |
| trace_id | string | Request that created this relationship |
| status | enum | active \| removed |
| created_at | timestamp | |
| created_by | string | |

### `operational_event`
[Transient actions against an existing CI that do not change its configuration or lifecycle. Operational offerings only.]

| Field | Type | Description |
|---|---|---|
| event_id | UUID | Surrogate PK |
| ci_id | UUID | FK → resource_ci.ci_id |
| trace_id | string | Request that triggered this operation |
| operation_type | string | restart \| index \| backup \| key-rotation \| drain \| flush-cache \| ... |
| initiated_by | string | |
| initiated_at | timestamp | |
| completed_at | timestamp | |
| outcome | enum | success \| failed \| timeout |
| evidence_uri | string | S3 path to the evidence package for this operation |
| detail | JSONB | Operation-specific metadata (reason, index name, duration, etc.) |

**Offering type → CMDB tables written:**

| Offering type | `resource_ci` | `provision_event` | `resource_relationship` | `operational_event` |
|---|---|---|---|---|
| `day1` | New record | `action: created` | No | No |
| `day2` | Sometimes | `modified` / `replaced` / `decommissioned` | Sometimes | No |
| `operational` | No | No | No | Yes |

## Evidence Package
[Contents written to S3 per ICD-008: canonical request, policy receipt, deployment
results. Storage path: `/audit-evidence/{Year}/{Month}/{trace_id}.json`.]

## Integration Contracts
[Mark each ICD applicable or not for this offering. Add offering-specific notes
for any contract parameters that differ from the default ICD schema.]

| ICD | Description | Applicable | Offering-specific notes |
|---|---|---|---|
| ICD-001 | RHDH → Orchestrator | Yes | |
| ICD-002 | Orchestrator → OPA | Yes | |
| ICD-003 | Orchestrator → HCP Terraform | Yes / No | |
| ICD-004 | HCP Terraform → Orchestrator | Yes / No | |
| ICD-005 | Orchestrator → EDA | Yes / No | |
| ICD-006 | EDA → AAP | Yes / No | |
| ICD-007 | AAP → Orchestrator | Yes / No | |
| ICD-008 | Orchestrator → S3 | Yes | |

## Resource Type
[Resource type(s) this offering provisions per
`.idea/library/standards/resource-type.md`.]

## Implementation Boundaries
**In scope:**
- [What this offering provisions and configures]

**Out of scope:**
- [What is explicitly not covered by this offering]

## Dependencies
- Compliance controls: `.idea/library/standards/`
- Source architecture: `architectures/<arch-id>/ref.md`

## Traceability
- architecture_id: <arch-id>
- activity_id: <ACTNNN>
- adr_artifact: <ADR file>
