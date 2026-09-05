---
name: "idea-list"
description: "Lists all architecture sessions and their current workflow stage, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "af6e1a8e5a2147590af1d56b9aacfbc77e9e66700d7e4000d51920d3be4a0f22"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/list.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/list.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

