---
activity_id: <ACTXXXXXX>
activity_name: <name>
lifecycle_domain: "Day 0 | Day 1 | Day 2 | Maintenance & Upgrade | Day N"
status: Draft
owner: Platform Engineering
architecture_id: <arch-id>
source_ref: <REFXXXXXX>
realizes_capability: <CAPXXXXXX>
source_ref_entry: <ACTNNN from ref.md>
created_date: <UTC date>
---

# <activity_name>

## Activity Description

This Lifecycle Activity represents a concrete, verb+object-phrased operation that realizes its parent capability.

**Operation Pattern**: Verb + Object (e.g., "Provision VM", "Resize VM", "Decommission VM")

**Lifecycle Domain**: [Select one]
- Day 0: Architecture & Infrastructure Provisioning
- Day 1: Application & Workload Deployment
- Day 2: Day-to-Day Platform Operations
- Maintenance & Upgrade Lifecycles
- Day N: Decommissioning & Teardown

**Reference**: See `.idea/library/instructions/lifecycle-operations.md` for patterns, domain examples, and anti-patterns.

## Realizes

This activity realizes capability `<realizes_capability>`.

## Description
[What this operation does, and what outcome it produces when performed]

## Success Criteria
[Measurable condition(s) that confirm this activity achieved its intended outcome]

## Dependencies
- Realized capability: `architectures/<arch-id>/<realizes_capability>.md`
- Source reference architecture: `architectures/<arch-id>/<source_ref>.md`
- Compliance controls: `.idea/library/standards/`

## Traceability
- source_architecture: architectures/<arch-id>/<source_ref>.md
- source_entry: <ACTNNN>
- realizes_capability: <realizes_capability>
