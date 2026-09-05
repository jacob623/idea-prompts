---
decision_id: <adrfile without .md>
title: "Architecture Decision — <selected option title>"
status: Draft
category: Platform Selection
owner: Enterprise Architecture
created_date: <UTC date>
---

# <title>

## Business Context

**Architecture**: <arch-id>
**Requirement**: <full requirement from session.json>
**Research artifact**: `<RES file used>`

## Problem Statement

Select the reference architecture for: <requirement summary in one sentence>.

## Decision Drivers
[Copied verbatim, in the same ranked order, from the source research artifact's `## Decision Drivers` section.]
1. [Driver 1]
2. [Driver 2]
3. [Driver 3]
...

## Requirements

- Compliance: [requirements from .idea/library/standards/ controls]
- Availability: RPO <effective RPO value>, RTO <effective RTO value> [NFR provenance: inherited/new/overridden; source and baseline when applicable]
- Throughput: <effective throughput value> [NFR provenance: inherited/new/overridden; source and baseline when applicable]
- Latency: <effective latency value> [NFR provenance: inherited/new/overridden; source and baseline when applicable]
- Data classification: <effective data classification> [NFR provenance: inherited/new/overridden; source and baseline when applicable]
- [If Q4a was triggered and architect requires ordered delivery]: Message ordering guaranteed per destination; FIFO queue required for applicable destinations.
- [If Q4a was triggered and at-least-once is acceptable]: At-least-once delivery accepted; application-level idempotency required at each consumer.
- [If Q6a was triggered and exactly-once is required]: Exactly-once delivery required; SQS FIFO queues and message deduplication IDs mandatory per destination.
- [If Q6a was triggered and at-least-once is acceptable]: At-least-once delivery accepted; application-level deduplication is the consumer's responsibility.
- [Additional requirements from any other interview answers]
- [For each overridden NFR: original research value, effective replacement, and change context]

## Options Considered

### <selected option ID>
[Summary from research output]
**Selected**: Yes

### <non-selected option 1 ID>
[Summary from research output]
**Rejected**: <reason drawn from compliance trade-off table or Stage 1 constraint answer>

### <non-selected option 2 ID>
[Summary from research output]
**Rejected**: <reason drawn from compliance trade-off table or Stage 1 constraint answer>

[Repeat for all remaining non-selected options]

## Selected Option

**<selected option ID>**

## Rationale

<EA's stated reasons from Stage 1, expanded with compliance alignment evidence from the research trade-off table>

## Consequences

**Positive**:
- [Benefits from the research summary for this option]

**Negative**:
- [Trade-offs and risks from the research]

**Operational**:
- [Operational requirements such as DLQ monitoring, concurrency caps, key rotation]

**Governance**:
- [Compliance obligations from .idea/library/standards/ including CSA CCM v4 controls, CIS benchmark hardening, IaC requirements]
- [If Q7 specifies BYOK or HSM-backed keys]: Customer-managed key custody obligation — BYOK/HSM-backed keys must be provisioned, rotated, and audited per the organisation's key management policy. Failure to rotate or revoke keys is a compliance finding.

## Reconsideration Triggers
[Conditions that, if they change, would require re-evaluating this decision, e.g., a compliance control changing, an NFR threshold being exceeded, or a preferred technology becoming unavailable]
- [Trigger 1]
- [Trigger 2]

## Economic Impact
[Cost, licensing, and operational overhead considerations behind this decision, drawn from the selected option's Economic Considerations in the source research artifact]

## Assumptions
[Conditions that must remain true for this decision to hold, e.g., inherited NFR values, BYOK/HSM requirements, preferred-technology availability]
- [Assumption 1]
- [Assumption 2]

## Dependencies

- Research artifact: `architectures/<arch-id>/<RES file>`
- Compliance controls: `.idea/library/standards/`

## Traceability

- originating_requirement: <requirement from session.json>
- research_artifact: <RES file used>
- architecture_id: <arch-id>
- nfr_sources: <session.json.nfrs fields used, newly supplied fields, and override records>
