---
research_id: <RESXXXXXX>
title: "<Descriptive Header reflecting the Reference Architecture>"
status: Draft
owner: Enterprise Architecture
architecture_id: <arch-id>
created_date: <UTC date>
---

# [Descriptive Header reflecting the Reference Architecture]

## Requirements Summary
[Restate the business requirement and the governing compliance controls in 2–3 sentences.
Include the workload NFRs captured during the interview:
- **Throughput**: [value, or "not specified"]
- **RPO (Recovery Point Objective)**: [value, or "not specified"]
- **RTO (Recovery Time Objective)**: [value, or "not specified"]
- **Latency**: [value, or "not specified"]
- **Data classification**: [value, or "not specified"]
- **Current architecture**: [REF path or description, or "New capability — no current architecture"]]

## Decision Drivers
[List the decision drivers from `decision-drivers.md` in their current ranked order, unmodified.]
1. [Driver 1]
2. [Driver 2]
3. [Driver 3]
...

## Recommendation
[State which architecture best meets the compliance controls and why, in 2–3 sentences.]

## Architecture Options
[List each reference architecture on one line as: `<resfile>-<Reference-Architecture>` where Reference-Architecture uses Title-Case-Hyphens. Suffix the single option named in `## Recommendation` with `**(recommended)**`; leave every other line unmarked. If every option is eliminated and `## Recommendation` states no viable option exists, leave every line unmarked.]
Example:
- RES000001-AWS-SQS-Lambda-Messaging **(recommended)**
- RES000001-Azure-Service-Bus-Functions

## Trade-off Comparison
| Criteria | Architecture 1 | Architecture 2 | ... |
|---|---|---|---|
| CSA CCM v4 | | | |
| CIS Benchmarks | | | |
| High Availability | | | |
| IaC/DSC Support | | | |
| Cloud-Native | | | |
| [Other relevant criteria] | | | |

## <resfile>-<Reference-Architecture>
**Unique ID**: `<resfile>-<Reference-Architecture>`

### Summary
[One paragraph describing how the architecture meets the business requirement and any technical advantages]

### Required Components
- Component 1
- Component 2
- ...

[One paragraph describing each component above.]

### Economic Considerations
[Cost profile, licensing considerations, and operational overhead for this option. If this option is eliminated for failing an NFR constraint, state `**Eliminated**: <failing constraint>` at the top of this subsection instead.]

[Repeat this section for each architecture.]

## Open Questions
[List assumptions or unknowns the architect should resolve before the architecture is finalised.]
- [Question 1]
- [Question 2]

## Traceability
- architecture_id: <arch-id>
- originating_requirement: <requirement>
- research_artifact: <RES file>
