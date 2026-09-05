---
name: "idea-imp"
description: "Creates a standalone Reference Implementation for an already-approved reference architecture, following a Direct-vs-Research-Driven decision flow; does not modify the reference architecture, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "61c9a7a9cc0e57e6d94c87ced18bcc762e4ec032bddc4e6eb052e892847d83e1"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/imp.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/imp.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

After all required work succeeds, update the `implementation` phase entry in `session.json` (adding it if absent) to `status: "approved"` with `approved_at` set to the current UTC timestamp. This is a historical record only: leave active_phase unchanged, do not require a user approval reply, and do not suggest a canonical next skill, since Implementation creation does not gate the main workflow.
