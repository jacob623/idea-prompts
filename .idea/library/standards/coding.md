# Coding Standard

## Purpose

The Coding Standard defines the implementation requirements,
boundaries, quality controls, traceability model, and governance
expectations for all implementation artifacts generated within the
Platform Engineering Architecture-to-Code lifecycle.

This standard establishes:

- Implementation responsibilities
- Code generation requirements
- Testing requirements
- Documentation requirements
- Traceability requirements
- Governance requirements
- Catalog evolution requirements

The purpose of this standard is to ensure all generated
implementations remain:

- Consistent
- Traceable
- Testable
- Governed
- Reproducible
- Maintainable

---

# Scope

This standard applies to:

- IaC Artifacts
- Workflow Definitions
- Policy Definitions
- Automation Definitions
- Software Templates
- Integration Implementations
- Database Schemas
- Configuration Artifacts
- Source Code
- Tests
- Documentation

---

# Architectural Principles

## CDS-1 Architecture Preservation

The Platform Coding Agent SHALL preserve the approved
Reference Architecture.

Implementation SHALL NOT modify:

- Capability Definitions
- Service Offerings
- Architecture Decisions
- Architecture Ownership Boundaries

## CDS-2 Solution Preservation

The Platform Coding Agent SHALL preserve the approved
Self-Service Solution.

Implementation SHALL NOT modify:

- Workflow Design
- Solution Scope
- Policy Intent
- Integration Intent

## CDS-3 Story-Based Implementation

Implementation SHALL occur only from approved Stories and Tasks.

## CDS-4 Deterministic Implementation

The same:

- Story
- Task
- Acceptance Criteria
- Standards Version

SHALL produce functionally equivalent implementation results.

## CDS-5 Traceability

All implementation artifacts SHALL remain traceable to:

Business Requirement
    ↓
Reference Architecture
    ↓
Self-Service Solution
    ↓
Agile Package
    ↓
Implementation

## CDS-6 Testability

Every implementation artifact SHALL be testable.

## CDS-7 Documentation

Every implementation artifact SHALL be documented.

## CDS-8 Artifact Types

The Platform Coding Agent SHALL support multiple implementation artifact types.

---

# Approved Inputs

The Platform Coding Agent SHALL consume:

- Stories
- Tasks
- Acceptance Criteria
- Approved Standards
- Approved Reference Architectures
- Approved Self-Service Solutions

The Platform Coding Agent SHALL NOT consume:

- Raw Requirements
- Draft Architectures
- Draft Stories
- Unapproved ADRs

---

# Traceability Requirements

Every implementation SHALL include:

traceability:

  reference_architecture:

  self_service_solution:

  agile_package:

  story_id:

  task_id:

  implementation_id:

Example:

traceability:

  reference_architecture:
    RA-001

  self_service_solution:
    SS-001

  agile_package:
    AP-001

  story_id:
    STORY-015

---

# Approved Artifact Types

The Platform Coding Agent MAY generate:

- IaC Artifacts
- Workflow Definitions
- Policy Definitions
- Automation Definitions
- Software Templates
- Integration Components
- Database Objects
- Configuration Objects
- Unit Tests
- Integration Tests
- Documentation

The Platform Coding Agent SHALL NOT generate:

- Reference Architectures
- ADRs
- Self-Service Solutions
- Agile Packages

---

# Repository Structure

## CDS-R1 Standard Layout

Generated implementation SHALL follow:

implementation/

  templates/

  iac-artifacts/

  workflows/

  policies/

  automation/

  integrations/

  configuration/

  tests/

  documentation/

## CDS-R2 Story Alignment

Artifacts SHALL be grouped according to the
originating Story.

---

# Naming Convention Requirements

## CDS-N1 Consistent Naming

Generated artifacts SHALL follow predictable naming conventions.

Examples:

restart-vm-template.yaml

restart-vm-workflow.yaml

restart-vm-policy.rego

restart-vm-job-template.yaml

restart-vm-contract.yaml

## CDS-N2 Service Offering Alignment

Artifact names SHOULD align with:

- Service Offering
- Story Name
- Implementation Type

---

# Acceptance Criteria Compliance

## CDS-A1 Mandatory Compliance

Every Story Acceptance Criterion SHALL be implemented.

## CDS-A2 Explicit Satisfaction

Implementations SHALL clearly demonstrate how acceptance criteria are met.

## CDS-A3 Missing Criteria

Implementation SHALL fail when Acceptance Criteria are absent.

---

# Testing Requirements

## CDS-T1 Test Generation

Every implementation SHALL generate tests.

## CDS-T2 Required Test Categories

Applicable test types include:

- Unit Tests
- Integration Tests
- Workflow Tests
- Policy Tests
- Contract Tests
- Security Tests

## CDS-T3 Positive Testing

Valid execution paths SHALL be tested.

## CDS-T4 Negative Testing

Failure conditions SHALL be tested.

## CDS-T5 Test Traceability

Tests SHALL reference the artifacts they validate.

---

# Documentation Requirements

## CDS-D1 Documentation Generation

Every implementation SHALL generate documentation.

## CDS-D2 Documentation Contents

Documentation SHALL include:

- Purpose
- Inputs
- Outputs
- Dependencies
- Acceptance Criteria Mapping
- Usage Guidance

## CDS-D3 Story Alignment

Documentation SHALL identify the Story being implemented.

---

# Security Requirements

## CDS-S1 Embedded Secret Prohibition

Implementations SHALL NOT contain:

- Passwords
- Tokens
- API Keys
- Certificates
- Encryption Keys

## CDS-S2 Externalized Secrets

Secrets SHALL be supplied through approved secret-management mechanisms.

## CDS-S3 Security Alignment

Implementations SHALL comply with approved security policies.

---

# Dependency Requirements

## CDS-DE1 Explicit Dependencies

Dependencies SHALL be documented.

## CDS-DE2 Hidden Dependency Prohibition

Hidden dependencies SHALL NOT be introduced.

## CDS-DE3 Dependency Traceability

Dependencies SHALL identify the consuming artifact.

---

# Resource Type Discovery

## CDS-RT1 Resource Type Lookup

Prior to proposing a new Resource Type the Platform Coding Agent SHALL:

- Search existing Resource Types
- Search Service Offerings
- Search Reference Architectures

## CDS-RT2 Resource Type Reuse

Existing Resource Types SHALL be reused whenever applicable.

## CDS-RT3 Resource Type Proposal

When no suitable Resource Type exists:

The Platform Coding Agent MAY propose a new Resource Type.

The proposed Resource Type SHALL be included within the generated change set.

## CDS-RT4 Resource Type Governance

New Resource Types SHALL require approval through the Git promotion process.

---

# Capability Discovery

## CDS-CAP1 Capability Lookup

The Platform Coding Agent SHALL identify the Capability associated with the implementation.

## CDS-CAP2 Capability Protection

Implementations SHALL NOT modify existing Capabilities.

## CDS-CAP3 Capability Proposal

When implementation exposes a missing Capability:

The Platform Coding Agent MAY recommend a new Capability.

The recommendation SHALL be reported separately from implementation.

---

# Service Offering Discovery

## CDS-SO1 Service Offering Lookup

The Platform Coding Agent SHALL identify the associated Service Offering.

## CDS-SO2 Reuse First

Existing Service Offerings SHALL be reused whenever possible.

## CDS-SO3 Proposal

When no suitable Service Offering exists:

The Platform Coding Agent MAY recommend a new Service Offering.

Recommendations SHALL be subject to approval through Git governance.

---

# GitOps Requirements

## CDS-G1 Version-Controlled Implementation

All generated artifacts SHALL be version-controlled.

## CDS-G2 Pull Request Model

All generated artifacts SHALL participate in:

- Branching
- Pull Requests
- Review
- Approval
- Promotion

## CDS-G3 Promotion

Implementation artifacts SHALL be promoted through approved environments.

Example:

Feature Branch
    ↓
Development
    ↓
QA
    ↓
Production

---

# Change Control

## CDS-CC1 Story Ownership

Implementation SHALL remain within Story scope.

## CDS-CC2 No Scope Expansion

The Platform Coding Agent SHALL NOT:

- Create new requirements
- Create new architecture decisions
- Create new architecture patterns

## CDS-CC3 Escalation

Missing requirements SHALL be escalated rather than invented.

---

# Quality Requirements

Generated implementations SHALL:

- Pass validation
- Pass tests
- Include documentation
- Satisfy acceptance criteria
- Maintain traceability
- Preserve architecture intent

---

# Definition of Done

Implementation SHALL NOT be considered complete until:

- Acceptance Criteria satisfied
- Tests generated
- Tests passing
- Documentation generated
- Traceability recorded
- Dependencies documented

---

# AI Agent Requirements

The Platform Coding Agent SHALL:

- Implement approved Stories
- Preserve architecture intent
- Preserve solution intent
- Generate tests
- Generate documentation
- Maintain traceability

The Platform Coding Agent SHALL NOT:

- Create Reference Architectures
- Modify ADRs
- Modify Self-Service Solutions
- Modify Agile Packages
- Invent requirements

---

# Compliance Requirements

Implementation artifacts SHALL NOT:

- Bypass governance
- Omit tests
- Omit documentation
- Omit traceability
- Introduce undocumented dependencies
- Introduce net-new architecture

All implementation artifacts SHALL remain traceable to an approved Story and Task and SHALL be eligible for review, promotion, and audit through the approved GitOps workflow.
## Platform Tooling Standards

### Terraform

All Terraform implementations SHALL use the public provider registry at
`registry.terraform.io`. Provider versions SHALL be pinned to exact versions
(e.g. `version = "5.31.0"`). Range constraints (`>=`, `~>`) are not permitted
outside development environments.

All Terraform state SHALL be managed in HCP Terraform remote backend. Local state
files (`.tfstate`) are not permitted in non-development environments.

### Database Migrations

All database schema changes SHALL be implemented as Flyway versioned migrations
(`V{version}__{description}.sql`). Raw DDL executed outside the migration
framework is not permitted in non-development environments. Migration scripts SHALL
be idempotent where technically feasible.
