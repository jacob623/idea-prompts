# Policy Standard

## Purpose

The Policy Standard defines authoritative governance rules used by Open Policy Agent (OPA) to make deterministic provisioning decisions.

This standard establishes policy structure, policy ownership, policy execution requirements, waiver handling, decision precedence, and policy lifecycle management.

All policy evaluations SHALL comply with this standard.

## Architectural Principles

### PS-1 Policy as Code

All governance controls SHALL be implemented as versioned policy code.

### PS-2 Deterministic Evaluation

The same:

- Canonical Request
- Policy Bundle Version
- Approved Waiver Set

SHALL produce the same decision.

### PS-3 Default Deny

Requests SHALL be denied whenever required evaluation criteria cannot be completed.

### PS-4 Explicit Ownership

Every policy SHALL have an assigned owner.

## Policy Categories

### Resource Policies

Examples:

- Resource sizing
- Resource quotas
- Environment restrictions

### Security Policies

Examples:

- Network controls
- Encryption requirements
- Security group validation

### Compliance Policies

Examples:

- Regulatory requirements
- Data residency controls
- Tagging requirements

### Operational Policies

Examples:

- Cost center validation
- Ownership validation
- Support model validation

## Policy Definition

Every policy SHALL define:

policy:
  id:
  name:
  version:
  owner:
  category:
  severity:
  description:
  evaluation_logic:
  remediation_guidance:
  approval_status:

## Policy Severity

Allowed Values:

- Critical
- High
- Medium
- Low

## Decision Outcomes

OPA SHALL return:

decision:
  Approved | Denied

No additional values are permitted.

## Conflict Resolution

### PD-1 Explicit Deny

Explicit Deny SHALL override Approve.

### PD-2 Most Restrictive Rule

More restrictive controls SHALL take precedence.

### PD-3 Ambiguous Evaluation

Ambiguous evaluations SHALL result in Denied.

### PD-4 Incomplete Data

Missing required attributes SHALL result in Denied.

## Waiver Standard

Every waiver SHALL contain:

waiver:
  waiver_id:
  policy_id:
  approver:
  justification:
  expiration_date:
  approval_timestamp:

Expired waivers SHALL NOT be evaluated.

## Policy Lifecycle

Allowed States:

- Draft
- Approved
- Deprecated
- Retired

Only Approved policies SHALL participate in evaluation.

## Policy Versioning

Policies SHALL follow Semantic Versioning.

Example:

1.0.0
1.1.0
2.0.0

## Audit Requirements

Every evaluation SHALL record:

policy_evaluation:
  trace_id:
  policy_bundle_version:
  evaluated_policies:
  decision:
  timestamp:

# Policy Implementation Requirements

## PS-I1 Policy Format

Policies SHALL be implemented as
version-controlled policy code.

## PS-I2 Policy Traceability

Policies SHALL identify:

- policy_id
- version
- owner

## PS-I3 Policy Testing

Policies SHALL include:

- Positive Tests
- Negative Tests
- Compliance Tests

## PS-I4 Policy Documentation

Policies SHALL include implementation documentation.
