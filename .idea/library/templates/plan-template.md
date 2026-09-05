---
plan_id: <OFRXXXXXX>-pkg-plan
offering_id: <OFRXXXXXX>
architecture_id: <arch-id>
status: Draft
created_date: <UTC date>
---

# Implementation Plan: <Offering Title>

**Package**: `architectures/<arch-id>/<OFRXXXXXX>-pkg/`

**Input**: Coding specification `architectures/<arch-id>/<OFRXXXXXX>-pkg/spec.md`

## Summary

[Primary implementation approach, in one or two sentences, grounded in the offering, activity,
and the ten standards read in Step 2a of `spec.md`.]

## Technical Context

**Language/Version**: [derived from the offering's provisioner/integration sections or
`coding.md`]

**Primary Dependencies**: [derived from the offering's Provisioner/Workflow Definition
sections]

**Resource Inventory**: [every resource type this implementation creates or modifies, per
`resource-type.md`]

**Runtime Assumptions**: [execution environment and state-machine assumptions, per
`workflow-state-machine.md` and `platform-architecture.md`]

**Constraints**: [domain-specific constraints surfaced by the standards read in `spec.md`]

## Implementation Areas

<!--
  ACTION REQUIRED: Add one "Implementation Area" section per area identified in spec.md's
  Implementation Scenarios & Testing section. Each area MUST define all ten subsections below —
  this is a hard requirement (FR-008), not an optional structure.
-->

### Implementation Area 1 - [Brief Title]

**Files & Modules**: [exact files and modules to create or modify, with paths]

**Resource Inventory**: [resources this area provisions or touches, per `resource-type.md`]

**Input/Output Schemas**: [request/response or configuration schemas this area consumes or
produces, per `data-architecture.md`]

**API & Integration Contracts**: [endpoints, canonical request mappings, or integration
contracts this area implements, per `canonical-request.md` and `platform-integration.md`]

**Dependency Sources & Versions**: [exact package/module sources and pinned versions]

**Authentication & Permissions**: [identity, roles, and access controls this area requires or
enforces, per `opa-policy.md`]

**Runtime Assumptions**: [configuration resolution, environment, and execution assumptions,
per `configuration-resolution.md` and `platform-architecture.md`]

**Error, Retry, and Idempotence Behavior**: [failure modes, retry policy, and idempotence
guarantees]

**Security Requirements**: [data classification, encryption, and policy gates this area must
satisfy, per `data-architecture.md` and `opa-policy.md`]

**Executable Validation Commands**: [exact runnable commands that prove this area works, per
`test.md`]

**Out-of-Scope Items**: [explicitly excluded from this area, per `implementation-boundary.md`]

---

[Repeat the Implementation Area section for each area in spec.md, incrementing the number.]

## Complexity Tracking

> Fill only if an implementation area cannot satisfy one of the ten required subsections above.

| Area | Missing Subsection | Why | Resolution |
|------|--------------------|-----|------------|
| [e.g., Area 2] | [e.g., Dependency Sources & Versions] | [reason] | [how/when it will be resolved] |
