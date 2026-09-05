---
name: "idea-tasks"
description: "Creates a task breakdown (<offerfile-id>-pkg/tasks.md) from the active session offering's spec package; an optional offering ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "d656e1134224869975ab409d8961c467255736d0246bc2c5e7b17aeac39e0df7"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/tasks.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/tasks.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

Derive `<offerfile-id>-pkg/` from the resolved offering (the same folder `/idea.spec` already writes to) under `architectures/<arch-id>/`.
Write your complete task breakdown to `<offerfile-id>-pkg/tasks.md`, overwriting it in place if it already exists.
The file must not be empty.
Required sections: Task Breakdown, Dependencies, Acceptance Criteria.
After all required work succeeds, mark the current session phase approved, record approved_at, advance active_phase, and suggest the canonical next skill. Do not require a user approval reply.
When suggesting the canonical next skill, render the complete invocation in a standalone fenced Markdown code block containing only the command invocation; keep labels, punctuation, and explanatory text outside the block.
Do not modify any other file as part of this command.
