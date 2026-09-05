# Capability Model Standard

## Purpose

The Capability Model Standard defines how business capabilities
are identified, structured, governed, and traced throughout the
Platform Engineering Architecture-to-Code lifecycle.

Capabilities SHALL serve as the primary mechanism for translating
business requirements into reference architectures and
self-service solutions.

This standard establishes:

- Capability identification
- Capability ownership
- Capability decomposition
- Capability dependencies
- Capability traceability
- Capability-to-solution mapping

---

# Architectural Principles

## CMS-1 Capability Driven Design

Reference Architectures SHALL be based upon business capabilities
rather than technologies, products, or implementation details.

## CMS-2 Technology Independence

Capabilities SHALL describe desired outcomes and SHALL NOT
describe implementation approaches.

## CMS-3 Single Capability Purpose

Each capability SHALL describe a single business outcome.

## CMS-4 Traceability

Capabilities SHALL remain traceable from:

Business Requirement
    ↓
Capability
    ↓
Reference Architecture
    ↓
Self-Service Solution
    ↓
Agile Package
    ↓
Implementation

## CMS-5 Reusability

Capabilities SHOULD be reusable across multiple
Reference Architectures.

---

# Capability Definition

Every capability SHALL contain:

capability:

  capability_id:
  capability_name:
  version:
  owner:
  status:

  business_outcome:

  description:

  consumers:

  dependencies:

  required_platforms:

  constraints:

  success_metrics:

  traceability:

---

# Capability Status

Allowed Values:

- Draft
- Proposed
- Approved
- Deprecated
- Retired

Only Approved capabilities SHALL be used in
Reference Architectures.

---

# Business Outcome

## CMS-B1 Outcome Focus

Every capability SHALL identify the business outcome it enables.

Example:

business_outcome:

  Allow application teams to request virtual
  machines through self-service automation.

Capabilities SHALL describe outcomes rather than processes.

GOOD:

- Self-Service Infrastructure Provisioning
- Network Security Policy Management
- Privileged Access Management

BAD:

- Terraform Module Deployment
- API Creation
- Workflow Execution

---

# Capability Ownership

## CMS-O1 Assigned Ownership

Every capability SHALL have an owner.

capability:

  owner:

Allowed Owners:

- Platform Engineering
- Identity Engineering
- Network Engineering
- Security Engineering
- Infrastructure Engineering
- Data Platform Team

Ownership SHALL represent accountability for capability outcomes.

---

# Capability Classification

Capabilities SHALL be classified using one of the following types:

- Business Capability
- Platform Capability
- Security Capability
- Compliance Capability
- Operational Capability

Example:

capability_type:
  Platform Capability

---

# Consumers

## CMS-C1 Consumer Definition

Capabilities SHALL identify intended consumers.

consumer:

  consumer_type:
  consumer_name:

Examples:

- Application Team
- Platform Engineer
- Network Engineer
- Security Engineer
- Database Administrator

---

# Capability Dependencies

## CMS-D1 Explicit Dependencies

Capabilities SHALL document dependencies.

dependencies:

  upstream:

  downstream:

Example:

Network Security Management

depends_on:

  - Identity Management
  - Policy Evaluation

---

# Required Platforms

## CMS-P1 Platform Identification

Capabilities SHALL identify participating platforms.

required_platforms:

  - RHDH
  - Orchestrator
  - OPA
  - HCP Terraform

Platforms SHALL be selected from approved
architecture standards.

---

# Constraints

## CMS-CT1 Constraint Definition

Capabilities SHALL document known constraints.

Examples:

- Regulatory controls
- Geographic restrictions
- Security requirements
- Technology standards

constraint:

  type:
  description:

---

# Success Metrics

## CMS-M1 Measurable Success

Every capability SHALL define measurable outcomes.

Examples:

success_metrics:

  - provisioning_time < 30 minutes

  - automation_rate > 90%

  - policy_compliance = 100%

---

# Capability Decomposition

## CMS-CD1 Parent Capability

Capabilities MAY be decomposed into child capabilities.

Example:

Self-Service Infrastructure

  ├── Virtual Machine Provisioning
  ├── Network Provisioning
  ├── Firewall Rule Management
  └── DNS Management

## CMS-CD2 Independent Value

Each child capability SHALL provide business value.

---

# Capability to Architecture Mapping

## CMS-A1 Architecture Generation

Reference Architectures SHALL be composed of capabilities.

Example:

Reference Architecture:

  Self-Service VM Platform

Capabilities:

  - VM Provisioning
  - Identity Validation
  - Policy Enforcement
  - Post-Provisioning Automation

---

# Capability to Solution Mapping

## CMS-S1 Solution Generation

Every Lifecycle Activity SHALL generate one or more
Self-Service Solution components. A capability commonly spans multiple
independently-solutioned lifecycle activities; the activity, not the
capability, is the atomic unit of solution generation.

Example:

VM Provisioning (capability)

    ↓

Provision VM (lifecycle activity)

    ↓

Software Template

    ↓

Resource Catalog Entry

    ↓

Terraform Module

    ↓

Workflow

---

# Capability to Agile Mapping

## CMS-G1 Delivery Mapping

Capabilities SHALL be decomposed into implementation work.

Example:

Capability:
    VM Provisioning

        ↓

Feature:
    VM Request Experience

        ↓

Story:
    Create VM Request Template

        ↓

Task:
    Implement Input Validation

---

# Traceability Requirements

Every capability SHALL contain:

traceability:

  originating_requirement:
  originating_stakeholder:
  reference_architectures:
  self_service_solutions:

## CMS-T1 Lifecycle Traceability

Authorized reviewers SHALL be able to trace:

Business Requirement
    ↓
Capability
    ↓
Architecture
    ↓
Solution
    ↓
Work Package
    ↓
Implementation

---

# AI Agent Requirements

The Reference Architecture Designer SHALL:

- Identify capabilities before defining solutions
- Normalize duplicate capabilities
- Reuse approved capabilities where possible
- Avoid technology-centric capabilities

The Self-Service Solution Designer SHALL:

- Consume approved capabilities
- Generate solution components from capabilities

The Agile Package Generator SHALL:

- Trace work items to capabilities

The Governance Reviewer SHALL:

- Validate capability traceability

---

# Example Capability

capability:

  capability_id:
    CAP-001

  capability_name:
    Self-Service Virtual Machine Provisioning

  capability_type:
    Platform Capability

  owner:
    Platform Engineering

  business_outcome:
    Allow development teams to provision approved
    virtual machines without manual intervention.

  consumers:

    - Application Teams

  required_platforms:

    - RHDH
    - Orchestrator
    - OPA
    - Terraform
    - EDA
    - AAP

  success_metrics:

    - provisioning_time < 30 minutes
    - automation_rate > 90%

  traceability:

    originating_requirement:
      Infrastructure Self-Service

---

# Compliance Requirements

Capabilities SHALL NOT:

- Describe implementation details
- Reference specific code artifacts
- Reference backlog items
- Reference technology products unless architecturally required
- Duplicate existing approved capabilities

All Reference Architectures SHALL be capability-driven and
traceable to approved capabilities.

# Capability Lifecycle Model

## CMS-L1 Reference Architecture Ownership

Capabilities SHALL be owned by a Reference Architecture.

Capabilities SHALL NOT exist independently of a Reference Architecture unless explicitly designated as enterprise-wide shared capabilities.

Capability metadata SHALL be maintained within the owning Reference Architecture.

Example:

reference-architectures/

  RA-001-workload-lifecycle-management/

    reference-architecture.md

    adrs/

      ADR-001.md

      ADR-002.md

The Reference Architecture SHALL contain the authoritative capability definition.

## CMS-L2 Capability Generation

When no suitable capability exists:

The Reference Architecture Designer SHALL:

- Create a new capability definition
- Associate the capability with the Reference Architecture
- Generate supporting ADRs

New capabilities SHALL be subject to normal Git review and approval processes.

## CMS-L3 Capability Reuse

The Reference Architecture Designer SHOULD reuse existing capabilities whenever practical.

The creation of duplicate capabilities SHALL be avoided.

## CMS-L4 Capability As Code

Capabilities SHALL be treated as version-controlled artifacts.

Capabilities SHALL support:

- Branching
- Pull Requests
- Review
- Approval
- Promotion