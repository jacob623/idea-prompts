---
ref_id: <REFXXXXXX>
title: "<Descriptive Reference Architecture Title>"
status: Draft
owner: Enterprise Architecture
architecture_id: <arch-id>
created_date: <UTC date>
---

<!--
Lifecycle Domains (fixed, ordered set used throughout this document):
- Day 0: Architecture & Infrastructure Provisioning — initial bootstrapping and deployment of underlying platform foundational resources.
- Day 1: Application & Workload Deployment — how application code and architecture components transition from source artifacts to running workloads.
- Day 2: Day-to-Day Platform Operations — continuous operations, scalability, and runtime platform maintenance.
- Maintenance & Upgrade Lifecycles — manages drift, vulnerabilities, and system evolution over time.
- Day N: Decommissioning & Teardown — safe removal of deprecated components or ephemeral environment cleanup.
-->

# [Descriptive Reference Architecture Title]

## Reusable Architecture Scope
[Describe the reusable architectural pattern, the underlying problem or capability it addresses, the stable architecture boundary, intended consumers, and what is deliberately generalized beyond the initiating business case.]

## Initiating Use Case Context
[Summarize the initiating business requirement and ADR context as a validation anchor. Label case-specific workflows, integrations, data fields, and constraints as context or examples unless a reusable rationale makes them part of the architecture.]

## Recommendation
[State the recommended approach and primary rationale in 2–3 sentences.]

## Components and Responsibilities
[Table or bulleted list: each component, its role, and its owner. Populate this list with the "Required Components" carried forward from the selected Research architecture option, plus any additional components the architect adds directly.]

## Interactions
[Describe how components interact at runtime: request flows, event triggers, data paths, and integration points.]

## Data Architecture
[For each data domain this architecture produces or consumes, declare: the authoritative
data store (per `.idea/library/standards/enterprise-architecture.md`), the data classification
tier (Public / Internal / Confidential / Restricted), the retention period, and the applicable
DAMA-DMBOK2 obligations from `.idea/library/standards/data-architecture.md`.]

## Assumptions and Constraints
[List the business, operational, and technical assumptions that were made, and any constraints the architecture must operate within.]

## Alternatives and Trade-offs
[Describe the alternatives considered and explain why the recommended approach was selected over each.]

## Risks
[Identify risks to delivery, operation, or adoption. Include likelihood, impact, and mitigation for each.]

## Security Considerations
[Describe how the architecture satisfies the applicable compliance controls from `.idea/library/standards/`. Include encryption, access control, audit, and network controls.]

## Operational Considerations
[Describe observability, deployment, scaling, and recovery requirements.]

## Validation Approach
[Describe how the architecture will be validated: tests, metrics, and criteria for declaring the architecture successfully implemented.]

## Applicability and Additional Use Cases
[State the applicability criteria and identify at least two additional-use-case categories or examples that can adopt this pattern. If no credible additional use cases are known, state the evidence gap and why reuse cannot yet be claimed.]

## Extension Points
[Describe where integrations, policies, workflows, and consumers can vary or be added without changing the stable architecture boundary.]

## Out-of-Scope Boundaries
[List explicit non-goals and cases that are not covered. Give the reason each boundary is not reusable or requires a separate architecture.]

## Uncertainty
[State unresolved assumptions, evidence gaps, and unsupported reuse claims. Identify what must be validated or reviewed before approval.]

## Implementations

### IMP1 — [Implementation Name]
**Implementation Objective**: [What this implementation approach accomplishes, e.g., "Provision compute via immutable images"]
**Satisfies**: [CAPNNN, ACTNNN, CTRLNNN, NFRNNN references from the sections above that this implementation satisfies]
**Platform**: [The target platform this implementation runs on, e.g., "AWS", "Azure", "On-Premises VMware"]
**Technology Mapping**:
- [Component] → [Technology, e.g., "Compute provisioning → Packer + HCP Terraform"]
- [Component] → [Technology]
**Implementation Approach**: [The concrete technical approach or pattern, e.g., "Golden image pipeline with automated CVE scanning"]
**Verification Method**: [How the implementation is verified as complete and correct, e.g., automated pipeline test]
**Assumptions**: [Conditions assumed to be true for this implementation to be valid]
**Constraints**: [Known limitations or boundaries this implementation must operate within]
**Platform-Specific Risks**: [Risks specific to the chosen technology/platform, e.g., vendor lock-in, version support window]

[Repeat IMPNNN entry for each additional implementation, incrementing the number.]

## NFRs

### NFR1 — [Requirement Name]
**Requirement Description**: [The measurable quality requirement, e.g., "API read latency at p99"]
**Target Value**: [The measurable target, e.g., "< 200ms"]
**Measurement Method**: [How the value is measured, e.g., synthetic monitoring, load test]
**Validation Method**: [How compliance with the target is validated, e.g., automated performance test]

[Repeat NFRNNN entry for each additional non-functional requirement, incrementing the number.
Populate entries from the architecture's inherited NFR values (research artifact /
session.json.nfrs) plus any architecture-specific additions.]

## Controls

### CTRL1 — [Control Name]
**Control Objective**: [What the control ensures is always true, e.g., "Data at rest is encrypted"]
**Control Requirement**: [The mandatory governance, compliance, security, or operational requirement itself]
**Verification Method**: [How compliance with this control is verified, e.g., audit, automated scan, review]
**Compliance References**: [Applicable standards or frameworks, e.g., CSA CCM v4, CIS Benchmarks]

[Repeat CTRLNNN entry for each additional control, incrementing the number.]

## Capabilities

### CAP001 — [Capability Name]
**Type**: Platform | Business | Security | Compliance
**Business Outcome**: [What users can accomplish when this capability is available]
**Consumers**: [User groups, teams, or systems that consume this capability]
**Service Offerings**:
- [User-visible action, e.g., "Provision a VM"]
- [User-visible action]
**Required Platforms**: [Platform names from enterprise standards, e.g., RHDH, Orchestrator, HCP Terraform]
**Constraints**: [Known limitations, governance requirements, or scope boundaries]

[Repeat CAPNNN entry for each additional capability, incrementing the number.]

## Lifecycle Activities

### ACT1 — [Activity Name]
**Lifecycle Domain**: [Select one: Day 0: Architecture & Infrastructure Provisioning | Day 1: Application & Workload Deployment | Day 2: Day-to-Day Platform Operations | Maintenance & Upgrade Lifecycles | Day N: Decommissioning & Teardown]
**Realizes**: [CAPNNN from the Capabilities section above]
**Success Criteria**: [Measurable condition(s) that confirm this activity achieved its intended outcome]

[Repeat ACTNNN entry for each additional lifecycle activity, incrementing the number. A capability may be realized by more than one activity; no minimum count per capability is enforced.]

## Dependencies
- Compliance controls: `.idea/library/standards/`
- Research artifact: `architectures/<arch-id>/<RES file>`

## Traceability
- architecture_id: <arch-id>
- originating_requirement: <requirement>
- adr_id: <ADR-ID>
- capability_sources: [Capability IDs and service offerings defined by this architecture]
