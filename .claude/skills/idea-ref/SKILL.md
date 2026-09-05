---
name: "idea-ref"
description: "Creates a reference architecture (ref.md) from the active session ADR indexed by the current architectures.yaml; an optional ADR ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "ceb3bcdc7e052ff9ca3d5464b47d286b8c1db748e5aa27729517cf564079cf1f"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/ref.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/ref.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

Write your complete response to `ref.md` at the workspace root.
Required sections: Reusable Architecture Scope, Initiating Use Case Context, Recommendation, Components and Responsibilities, Interactions, Data Architecture, Assumptions and Constraints, Alternatives and Trade-offs, Risks, Security Considerations, Operational Considerations, Validation Approach, Applicability and Additional Use Cases, Extension Points, Out-of-Scope Boundaries, Uncertainty, Capabilities, Lifecycle Activities, Dependencies, Traceability.
After all required work succeeds, mark the current session phase approved, record approved_at, advance active_phase, and suggest the canonical next skill. Do not require a user approval reply.
When suggesting the canonical next skill, render the complete invocation in a standalone fenced Markdown code block containing only the command invocation; keep labels, punctuation, and explanatory text outside the block.
Do not modify any other file as part of this command.
