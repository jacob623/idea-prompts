---
name: "idea-spec"
description: "Creates a coding specification and implementation plan package (<offerfile-id>-pkg/spec.md, plan.md) from the active session offering; an optional offering ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "a8f2415cb2cf21b857f49fc0c93a4c97db040a3fa11cb202afa0045c1677b100"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/spec.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/spec.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

Following the artifact-numbering rule in `architecture-index.md`, derive `<offerfile-id>-pkg/` from the resolved offering and create it under `architectures/<arch-id>/`.
Write your complete coding specification to `<offerfile-id>-pkg/spec.md` and your complete implementation plan to `<offerfile-id>-pkg/plan.md`, both under `architectures/<arch-id>/`.
Neither file must be empty.
After all required work succeeds, mark the current session phase approved, record approved_at, advance active_phase, and suggest the canonical next skill. Do not require a user approval reply.
When suggesting the canonical next skill, render the complete invocation in a standalone fenced Markdown code block containing only the command invocation; keep labels, punctuation, and explanatory text outside the block.
Do not modify any other file as part of this command.
