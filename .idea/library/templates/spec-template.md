---
spec_id: <OFRXXXXXX>-pkg-spec
offering_id: <OFRXXXXXX>
architecture_id: <arch-id>
status: Draft
created_date: <UTC date>
---

# Coding Specification: <Offering Title>

**Package**: `architectures/<arch-id>/<OFRXXXXXX>-pkg/`

**Input**: Offering `architectures/<arch-id>/<OFRXXXXXX>.md`, activity
`architectures/<arch-id>/<activity-artifact>` (or `ref.md` if the activity phase was skipped)

## Implementation Scenarios & Testing

### Implementation Area 1 - [Brief Title] (Priority: P1)

[Describe what this implementation area delivers, in plain language, grounded in the offering
and activity content above.]

**Why this priority**: [Explain the value and why it has this priority level.]

**Independent Test**: [Describe how this area can be validated independently once implemented.]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. **Given** [initial state], **When** [action], **Then** [expected outcome]

---

[Add more implementation areas as needed, each with an assigned priority. Every area listed
here must have a matching Implementation Area section in `plan.md`.]

### Edge Cases

- What happens when [boundary condition]?
- How does the implementation handle [error scenario]?

## Requirements

### Functional Requirements

- **FR-001**: The implementation MUST [specific capability derived from the offering/activity]
- **FR-002**: The implementation MUST [specific capability]

*Example of marking unclear requirements:*

- **FR-003**: The implementation MUST [behavior] for [NEEDS CLARIFICATION: detail not resolved
  by the offering or activity]

### Key Entities

- **[Entity 1]**: [What it represents, key attributes, without implementation detail]
- **[Entity 2]**: [What it represents, relationships to other entities]

## Success Criteria

### Measurable Outcomes

- **SC-001**: [Measurable, technology-agnostic outcome]
- **SC-002**: [Measurable, technology-agnostic outcome]

## Assumptions

- [Assumption made where the offering or activity did not specify a detail, with rationale]
