---
name: "idea-adr"
description: "Creates an Architecture Decision Record from the active session research artifact indexed by the current architectures.yaml; an optional research ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "61c9a7a9cc0e57e6d94c87ced18bcc762e4ec032bddc4e6eb052e892847d83e1"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/adr.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/adr.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

After all required work succeeds, mark the current session phase approved, record approved_at, advance active_phase, and suggest the canonical next skill. Do not require a user approval reply.
When suggesting the canonical next skill, render the complete invocation in a standalone fenced Markdown code block containing only the command invocation; keep labels, punctuation, and explanatory text outside the block.
