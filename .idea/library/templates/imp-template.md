---
imp_id: <IMPXXXXXX>
implementation_name: <name>
status: Draft
owner: Enterprise Architecture
architecture_id: <arch-id>
source_ref_entry: <IMPNNN from ref.md>
created_date: <UTC date>
---

# <implementation_name>

## Implementation Objective
[What this implementation approach accomplishes, e.g., "Provision compute via immutable images"]

## Architecture Compliance
[CAPNNN, ACTNNN, CTRLNNN, NFRNNN references from the source Reference Architecture that this implementation satisfies]

## Platform
[The target platform this implementation runs on, e.g., "AWS", "Azure", "On-Premises VMware"]

## Technology Mapping
- [Component] → [Technology]
- [Component] → [Technology]

## Implementation Approach
[The concrete technical approach or pattern, e.g., "Golden image pipeline with automated CVE scanning"]

## Verification Method
[How the implementation is verified as complete and correct, e.g., automated pipeline test]

## Assumptions
[Conditions assumed to be true for this implementation to be valid]

## Constraints
[Known limitations or boundaries this implementation must operate within]

## Platform-Specific Risks
[Risks specific to the chosen technology/platform, e.g., vendor lock-in, version support window]

## Dependencies
- Source architecture: `architectures/<arch-id>/ref.md`

## Traceability
- source_architecture: architectures/<arch-id>/ref.md
- source_entry: <IMPNNN>
