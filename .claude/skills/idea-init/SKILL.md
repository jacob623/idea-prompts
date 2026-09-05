---
name: "idea-init"
description: "Creates the initial task artifact (init.md) from a task description, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "af6e1a8e5a2147590af1d56b9aacfbc77e9e66700d7e4000d51920d3be4a0f22"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/init.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/init.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
## Output Contract

Write your complete response to `init.md` at the workspace root.
The file must not be empty.
Do not modify any other file as part of this command.
