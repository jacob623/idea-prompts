# Decision Driver Standard

## Purpose

The Decision Driver Standard defines the ranked priorities used to evaluate and compare
architecture and implementation alternatives during Research and to record the selected
rationale in an Architecture Decision Record.

This standard establishes:

- The ranked list of decision drivers
- How the ranking is used to frame trade-off comparisons
- How the ranking is inherited from Research into an ADR

---

# Architectural Principles

## DDS-1 Ranked Evaluation

Research SHALL evaluate architecture alternatives against the decision drivers below in their
current ranked order, giving comparison weight to higher-ranked drivers first.

## DDS-2 Decision Inheritance

An ADR SHALL record the identical ranked decision drivers captured by its source Research
artifact. An ADR SHALL NOT re-derive or re-rank decision drivers independently.

## DDS-3 Organization-Specific Priorities

This ranked list is workspace-global and MAY be reordered, renamed, expanded, or reduced by
editing this file directly. No other file needs to change for a re-prioritization to take
effect on the next `/idea.research` run.

---

# Decision Drivers

Ranked from highest to lowest priority:

1. Compliance
2. Recoverability
3. Operational Simplicity
4. Scalability
5. Performance
6. Cost
