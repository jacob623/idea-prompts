---
name: "idea-codify"
description: "Reads the active session task breakdown and executes each task, writing codify.md; an optional task ID may select a specific artifact, visibly in chat."
metadata:
  source: "idea init / idea sync-prompts"
  generated_from_fingerprint: "bd149a8d84a1dfa0068f54e95d4a2006b97fe0f783e0fec28e6a5ddfd941af72"
---

## User Input

```text
$ARGUMENTS
```

## Skill Instructions

Read `.idea/library/instructions/codify.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/codify.md`. Run `/idea.init` to restore the instructions library, then run this skill again."
For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly. If the file is absent, tell the user: "Instruction file not found at `.idea/library/instructions/architecture-index.md`. Run `/idea.init` to restore the instructions library, then run this skill again."

## Output Contract

Resolve the active offering's `<offerfile-id>-pkg/` folder and read `tasks.md`, `spec.md`, and `plan.md` from it yourself. For each checklist line (`- [ ]` or `- [x]`) in `tasks.md`, execute the task and append a `## Task N` section to `codify.md` describing what was done, in order.
The file must contain at least one `## Task` section.
Write the complete result to `codify.md` at the workspace root.
After all required work succeeds, mark the current session phase approved, record approved_at, advance active_phase, and suggest the canonical next skill. Do not require a user approval reply.
When suggesting the canonical next skill, render the complete invocation in a standalone fenced Markdown code block containing only the command invocation; keep labels, punctuation, and explanatory text outside the block.
Do not modify any other file outside what each task requires.
