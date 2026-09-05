# Agile Delivery Standard

## Purpose

The Agile Delivery Standard defines how Self-Service Solutions are
transformed into implementation work packages.

This standard establishes:

- Work decomposition
- Traceability requirements
- Artifact ownership
- Dependency management
- Acceptance criteria structure
- Definition of Done

The Agile Package SHALL be sufficiently detailed to enable
implementation by an AI coding agent or engineering team.

---

# Architectural Principles

## ADS-1 Traceable Delivery

Every Epic, Feature, Story, and Task SHALL be traceable back to
the originating Self-Service Solution component.

## ADS-2 Deterministic Decomposition

The same Self-Service Solution SHALL generate the same Agile Package.

## ADS-3 Single Responsibility

Each Story SHALL represent a single implementation deliverable.

## ADS-4 Completion Verification

Every Story SHALL define objective acceptance criteria.

## ADS-5 Dependency Visibility

Dependencies SHALL be explicitly represented.

## ADS-6 Incremental Delivery

Work SHALL be decomposed into independently verifiable increments.

---

# Work Hierarchy

The approved hierarchy SHALL be:

Epic
  └── Feature
          └── Story
                  └── Task

No additional hierarchy levels SHALL be introduced.

---

# Epic Definition

An Epic represents an implementation domain.

epic:
  epic_id:
  title:
  objective:
  success_criteria:
  source_components:

Examples:

- Template Implementation
- Configuration Management
- Policy Enforcement
- Provisioning Automation
- Post-Provisioning Automation
- Observability
- Evidence Management

---

# Feature Definition

A Feature represents a deployable capability.

feature:
  feature_id:
  epic_id:
  title:
  description:
  acceptance_criteria:
  source_component:

Examples:

- VM Request Template
- OPA Policy Bundle
- Terraform VM Provisioning

---

# Story Definition

A Story represents an independently deliverable implementation item.

story:
  story_id:
  feature_id:
  title:
  implementation_type:
  description:
  source_component:
  acceptance_criteria:
  dependencies:
  deliverables:

---

# Task Definition

A Task represents a specific implementation activity.

task:
  task_id:
  story_id:
  title:
  task_type:
  description:
  acceptance_criteria:
  dependencies:

---

# Approved Implementation Types

Stories SHALL define one of the following implementation types:

- software-template
- resource-catalog-entry
- configuration-profile
- configuration-resolution
- canonical-mapping
- policy
- workflow
- implementation-artifact
- automation-catalog-entry
- automation-job
- integration-contract
- database-schema
- observability
- evidence-management
- documentation
- testing

---

# Approved Task Types

Tasks SHALL define one of the following task types:

- design
- development
- configuration
- terraform
- policy
- workflow
- automation
- integration
- database
- testing
- documentation
- deployment

---

# Work Decomposition Rules

## ADS-R1 Template Rule

A Software Template SHALL generate at least:

- One Feature
- One Story

## ADS-R2 Resource Catalog Rule

A Resource Catalog Entry SHALL generate at least:

- One Story

## ADS-R3 Policy Rule

Each Policy SHALL generate at least:

- One Story
- One Policy Test Task

## ADS-R4 Implimentation Artifact Rule

Each Implementation Artifact SHALL generate at least:

- One Story
- One Testing Task
- One Documentation Task

Implementation Artifacts SHALL identify:

- artifact_type
- artifact_name

## ADS-R5 Workflow Rule

Each Workflow SHALL generate at least:

- One Story
- One Workflow Test Task

## ADS-R6 Automation Rule

Each Automation Catalog Entry SHALL generate at least:

- One Story

## ADS-R7 Integration Rule

Each Integration Contract SHALL generate at least:

- One Story
- One Integration Test Task

---

# Dependency Rules

## ADS-D1 Explicit Dependencies

All dependencies SHALL be represented.

## ADS-D2 Circular Dependencies

Circular dependencies SHALL NOT be generated.

## ADS-D3 Cross-Epic Dependencies

Cross-Epic dependencies SHALL be explicitly identified.

dependency:
  dependency_id:
  dependency_type:
  source_work_item:
  target_work_item:
  rationale:

Allowed dependency types:

- blocks
- requires
- relates_to

---

# Acceptance Criteria Rules

## ADS-A1 Objective Validation

Acceptance Criteria SHALL be:

- Testable
- Measurable
- Unambiguous
- Independently verifiable

## ADS-A2 Implementation Independence

Acceptance Criteria SHALL describe outcomes rather than implementation methods.

GOOD:

- Terraform validate completes successfully.
- OPA returns Approved for compliant requests.
- Software Template validates required parameters.

BAD:

- Terraform code looks correct.
- Policy works correctly.
- Integration behaves as expected.

## ADS-A3 Completion Verification

Every Story SHALL contain at least one acceptance criterion.

Every Task SHALL contain at least one acceptance criterion.

---

# Deliverable Requirements

## ADS-DR1 Deliverable Declaration

Every Story SHALL declare the implementation artifacts that will be produced.

Example:

deliverables:
  - software_template.yaml
  - documentation.md
  - tests/

## ADS-DR2 Explicit Outputs

Implementation outputs SHALL NOT be implied.

All expected artifacts SHALL be listed.

---

# Traceability Requirements

## ADS-T1 Solution Traceability

Every work item SHALL reference its originating Self-Service Solution component.

traceability:
  source_solution:
  source_component:
  source_artifact:

## ADS-T2 Architecture Traceability

Work items SHOULD retain linkage back to the originating Reference Architecture capability.

traceability:
  source_capability:
  source_solution:
  source_component:

## ADS-T3 End-to-End Traceability

Authorized reviewers SHALL be able to trace:

Reference Architecture
  ↓
Self-Service Solution
  ↓
Epic
  ↓
Feature
  ↓
Story
  ↓
Task
  ↓
Implementation Artifact

---

# Epic Generation Rules

## ADS-E1 Capability Mapping

Each major solution capability SHALL generate an Epic.

Examples:

- Request Experience
- Policy Enforcement
- Provisioning
- Automation
- Observability
- Audit and Evidence

## ADS-E2 Epic Scope

Epics SHALL represent implementation domains and SHALL NOT represent individual tasks.

---

# Feature Generation Rules

## ADS-F1 Feature Scope

Features SHALL represent deployable capabilities.

Examples:

- VM Request Template
- VM Provisioning Workflow
- VM Policy Bundle
- VM Automation Catalog

## ADS-F2 Feature Ownership

Each Feature SHALL belong to exactly one Epic.

---

# Story Generation Rules

## ADS-S1 Single Deliverable Rule

Each Story SHALL produce one primary implementation outcome.

Examples:

GOOD:

- Create VM Software Template
- Create VM Terraform Module
- Create VM Policy Bundle

BAD:

- Build Complete VM Platform

## ADS-S2 Implementation Type

Every Story SHALL define exactly one implementation type.

story:
  implementation_type:

Allowed Values:

- software-template
- resource-catalog-entry
- configuration-profile
- configuration-resolution
- canonical-mapping
- policy
- implementation-artifact
- workflow
- integration-contract
- automation-catalog-entry
- automation-job
- observability
- evidence-management
- documentation
- testing

## ADS-S3 Story Deliverables

Every Story SHALL declare deliverables.

Example:

story:

  story_id: STORY-021

  deliverables:
    - vm-template.yaml
    - README.md
    - validation-tests/

---

# Task Generation Rules

## ADS-G1 Task Scope

Tasks SHALL represent individual implementation activities.

## ADS-G2 Task Ownership

Tasks SHALL belong to exactly one Story.

## ADS-G3 Task Granularity

Tasks SHOULD be completable without requiring decomposition into additional tasks.

Example:

GOOD:

- Add input validation
- Create Terraform outputs
- Implement OPA rule

BAD:

- Build Platform

---

# Testing Requirements

## ADS-Q1 Testing Requirement

Every implementable Story SHALL generate testing work.

## ADS-Q2 Approved Test Types

Allowed test types:

- unit-test
- integration-test
- workflow-test
- policy-test
- contract-test
- security-test

## ADS-Q3 Test Traceability

Every test SHALL reference the Story it validates.

---

# Definition of Done

A Story SHALL NOT be marked complete until:

- Implementation completed
- Acceptance criteria satisfied
- Documentation completed
- Testing completed
- Governance requirements satisfied
- Dependencies resolved

definition_of_done:

  implementation_complete: true
  acceptance_criteria_complete: true
  tests_complete: true
  documentation_complete: true
  governance_review_complete: true

---

# Agile Package Metadata

Every Agile Package SHALL contain:

package_metadata:

  package_name:
  package_version:
  source_solution:
  source_solution_version:
  generated_timestamp:
  generated_by:

---

# Compliance Requirements

The Agile Package Generator SHALL NOT:

- Generate work that cannot be traced to the Self-Service Solution.
- Omit acceptance criteria.
- Omit dependencies.
- Omit deliverables.
- Generate circular dependencies.
- Combine unrelated capabilities into a single Story.
- Generate implementation work outside the approved solution scope.

The Agile Package SHALL serve as the implementation contract between:

- Self-Service Solution Designer
- Agile Package Generator
- Platform Coding Agent
- Governance Reviewer