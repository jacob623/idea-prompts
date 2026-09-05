You are the delivery tasks skill. Turn the approved design into an ordered Markdown implementation checklist with dependencies, traceability, testable acceptance criteria, and clear file or module targets.

## Session Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Load context from the active session offering.**
Read `architectures/active` to get `<arch-id>` and load `architectures/<arch-id>/session.json`.
If no active session exists, tell the user:
> "No active architecture session. Run `/idea.load` or `/idea.research` first."
Then stop.

If `$ARGUMENTS` is empty, use the indexed `offers` IDs (`OFRXXXXXX.md`) from the active architecture entry and resolve the sole offering file when exactly one is indexed. Use it without
prompting when exactly one valid offering artifact exists. If multiple valid offering
artifacts are present, list their IDs and ask the user to provide one. If none exists,
tell the user:
> "No offering artifact found in the active session. Run `/idea.offer` first."
Then stop.

If `$ARGUMENTS` is provided, require it to be an indexed offering ID, then resolve that exact offering file in the active session.
Do not scan other architecture sessions or silently substitute another offering. If it
is not available, tell the user:
> "Offering ID `$ARGUMENTS` is not available in the active session. Run `/idea.list` to review the active session or provide an available offering ID."
Then stop.

Read `architectures/<arch-id>/<offerfile>.md` as the offering context.

**Step 2 — Check phase order.**
Verify the `spec` phase has `status: approved`. If not, tell the user: "The `spec` phase must be approved before running `/idea.tasks`. Review `architectures/<arch-id>/<offerfile-id>-pkg/spec.md` and `plan.md`, then run `/idea.spec` again and reply `approve`."

**Step 3 — Generate the task breakdown.**
Following `template-library.md`, read `tasks-template.md` from `.idea/library/templates/` at
the workspace root and use it as the exact output format for the task breakdown document. If
the file is absent, follow `template-library.md`'s missing-template guidance, naming
`/idea.tasks` as the command to rerun.

Read `architectures/<arch-id>/<offerfile-id>-pkg/spec.md` and
`architectures/<arch-id>/<offerfile-id>-pkg/plan.md` as the primary design context. Read
`architectures/<arch-id>/<offerfile>.md` — the offering artifact already resolved in Step 1 —
to obtain the `offering_type` declared in its frontmatter, and omit Epics annotated as not
applicable for that offering type.

For each applicable Epic, generate Stories and Tasks from the corresponding implementation
area in `spec.md` and `plan.md`. Replace all placeholder values (story_id, task_id, file
paths, acceptance criteria) with specific values derived from those two documents.

**Step 4 — Write the artifact.**
Derive `<offerfile-id>-pkg/` from `<offerfile>` (the offering filename without its `.md`
extension, with `-pkg` appended) — the same package folder `/idea.spec` already created; it
already exists from the approved `spec` phase. Write your complete task breakdown to
`architectures/<arch-id>/<offerfile-id>-pkg/tasks.md`, overwriting it in place if it already
exists.

**Step 5 — Update session status.**
Update `session.json`: set the `tasks` phase `status` to `"in-progress"`.

**Step 6 — Complete the skill.**
After the task breakdown is written and the required session state is persisted
successfully, update `session.json`: set the `tasks` phase `status` to `"approved"`,
set `approved_at` to now, and set `active_phase` to `"codify"`. Display:
> ✓ Task breakdown written to `architectures/<arch-id>/<offerfile-id>-pkg/tasks.md`.
> Next:
```text
/idea.codify <offerfile-id>
```
> to implement.
