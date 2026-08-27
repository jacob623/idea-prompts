# Canonical Mapping Standard

## Purpose

The Canonical Mapping Standard defines how enterprise configuration data and user-supplied request data are incorporated into the Canonical Request.

This standard establishes authoritative mappings used during Canonical Request assembly.

## Architectural Principles

### CMS-1 Explicit Mapping

Every enterprise configuration attribute SHALL define an approved Canonical Request destination.

### CMS-2 Mapping Authority

Canonical Mappings SHALL be maintained as configuration data.

### CMS-3 No Inference

Workflow implementations SHALL NOT infer Canonical Request mappings.

### CMS-4 Mapping Consistency

The same source attribute SHALL always populate the same Canonical Request destination.

### CMS-5 Fail Closed

Required mapping failures SHALL halt workflow execution.

## Canonical Mapping Model

canonical_mapping:
  mapping_id:
  resource_type:
  source_attribute:
  target_path:
  required:

## Canonical Request Sources

### User Request Data

User-supplied values SHALL populate:

resource.requested_configuration

unless otherwise documented.

### Identity Context

Identity values SHALL populate:

requester

using Okta as the authoritative source.

### Template Context

Template metadata SHALL populate:

template

using RHDH as the authoritative source.

### Enterprise Configuration

Resolved enterprise configuration SHALL populate:

resource.resolved_configuration

using approved Canonical Mappings.

### Configuration Context

Configuration metadata SHALL populate:

configuration_context

using PostgreSQL as the authoritative source.

## Canonical Request Assembly Sequence

The Orchestrator SHALL construct the Canonical Request using:

- User Request Data
- Identity Context
- Template Metadata
- Enterprise Configuration
- Configuration Context

Resulting in:

canonical_request:
  trace_id:
  traceparent:
  request_id:

  requester:

  template:

  target:

  resource:
    requested_configuration:
    resolved_configuration:

  configuration_context:

  governance:

  submission:

## Mapping Processing Rules

### CMS-R1 Mapping Retrieval

The Orchestrator SHALL load Canonical Mappings associated with the Resource Type.

### CMS-R2 Mapping Validation

Every Required mapping SHALL be validated.

### CMS-R3 Mapping Completeness

All Required Canonical Request paths SHALL be populated prior to Policy Evaluation.

### CMS-R4 Mapping Conflict

When multiple mappings target the same Canonical Request destination:

- Assembly SHALL fail.
- Workflow SHALL halt.

### CMS-R5 Null Values

Required Canonical Request destinations SHALL NOT contain null values.

## Failure Handling

### CMS-F1 Assembly Failure

Mapping failures SHALL prevent Canonical Request construction.

### CMS-F2 Policy Protection

Policy Evaluation SHALL NOT begin until Canonical Request construction succeeds.

### CMS-F3 Provisioning Protection

Provisioning SHALL NOT begin until Canonical Request construction succeeds.

## Audit Requirements

### CMS-A1 Traceability

Mappings used during execution SHALL remain associated with the originating Trace_ID.

### CMS-A2 Reproducibility

Authorized auditors SHALL be able to reconstruct the Canonical Request used during execution.

### CMS-A3 Historical Preservation

Historical Canonical Requests SHALL remain associated with the Configuration Profile Version used at execution time.

## Compliance Requirements

Workflow implementations SHALL NOT:

- Hardcode Canonical Request mappings.
- Infer Canonical Request destinations.
- Modify resolved configuration after Canonical Request assembly.
- Bypass mapping validation.

All Canonical Request construction SHALL use approved Canonical Mapping definitions.