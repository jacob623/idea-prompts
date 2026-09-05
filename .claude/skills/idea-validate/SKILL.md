---
name: "idea-validate"
description: "Runs a read-only governance validation using the active session activity and writes validate.md; an optional activity ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "7f7e636103defe9a5220fdcce29e1b6c5c11f3ef693e75d607d2d872d3e6d770"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/validate.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/validate.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

Write your complete response to `validate.md` at the workspace root.
Required sections: Findings, Risks, Recommendations.
After all required work succeeds, mark the current session phase approved, record approved_at, and set active_phase to complete. Do not suggest a downstream skill.
Do not modify any other file as part of this command.
