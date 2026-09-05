You are the implementation skill. Complete one approved task in the active workspace. Preserve upstream artifacts, follow applicable delivery standards, run validation, and report the change and evidence as Markdown.

## Session Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Load context from the active session offering.**
Read `architectures/active` to get `<arch-id>` and load `architectures/<arch-id>/session.json`.
If no active session exists, tell the user:
> "No active architecture session. Run `/idea.load` or `/idea.research` first."
Then stop.

If `$ARGUMENTS` is empty, use the indexed `offers` IDs (`OFRXXXXXX.md`) from the active
architecture entry and resolve the sole offering file when exactly one is indexed. Use it without
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

Read `architectures/<arch-id>/<offerfile>.md` as the offering context. Derive
`<offerfile-id>-pkg/` from `<offerfile>` (the offering filename without its `.md` extension,
with `-pkg` appended) and confirm `architectures/<arch-id>/<offerfile-id>-pkg/tasks.md` exists.
If it does not, tell the user:
> "No task breakdown found in `<offerfile-id>-pkg/`. Run `/idea.tasks` first."
Then stop.

**Step 2 — Check phase order.**
Verify the `tasks` phase has `status: approved`. If not, tell the user: "The `tasks` phase must be approved before running `/idea.codify`. Review `architectures/<arch-id>/<offerfile-id>-pkg/tasks.md` and run `/idea.tasks` to approve."

**Step 3 — Execute the tasks.**
Read `architectures/<arch-id>/<offerfile-id>-pkg/tasks.md`,
`architectures/<arch-id>/<offerfile-id>-pkg/spec.md`, and
`architectures/<arch-id>/<offerfile-id>-pkg/plan.md`. If any of the three cannot be read, tell
the user which specific file is missing and stop without generating any artifact.
Use `spec.md` and `plan.md` as the design specification: each task's `source_component` field
identifies the `plan.md` implementation area or `spec.md` requirement that provides the
field-level detail for code generation (policy rules, Terraform variables, workflow steps,
Flyway schema, etc.).

For each checklist line (`- [ ]` or `- [x]`) in `tasks.md`:
1. Write the code artifact to the file path specified in the task, creating
   directories as needed. Use the file extension for the task's `task_type`:
   `policy` → `.rego`; `software-template` → `.yaml`; `workflow` → `.sw.yaml`;
   `terraform` → `.tf`; `automation-job` → `.yml`; `database-schema` → `.sql`;
   `testing` → extension matches artifact under test; `documentation` → `.md`.
2. Run the applicable verification for the `task_type` (e.g. `rego test` for policy,
   `terraform validate` for terraform, `flyway validate` for database-schema).
3. Record the artifact path, task_type, and verification result for the report.

If a task cannot be completed (missing source detail, tool unavailable), record it in
the Unresolved Items section with a reason rather than skipping silently.

**Step 4 — Write the implementation report.**
Following `template-library.md`, read `codify-template.md` from `.idea/library/templates/` at
the workspace root and use it as the exact output format for the implementation report. If the
file is absent, follow `template-library.md`'s missing-template guidance, naming
`/idea.codify` as the command to rerun.

Populate the template from the execution results in Step 3. Following the artifact-numbering
rule in `architecture-index.md`, derive the next `CDYXXXXXX.md` filename in
`architectures/<arch-id>/`. This is `<codifyfile>`.
Populate the template from the execution results in Step 3. Write the completed report
to `architectures/<arch-id>/<codifyfile>`.

**Step 4a — Record the code artifact in the architecture index.**
Following the write procedure in `architecture-index.md`, append `<codifyfile>` (without its `.md` extension) to the `code` list of the `<arch-id>` entry in `architectures.yaml`, if not already present.

**Step 5 — Update session status.**
Update `session.json`: set the `codify` phase `status` to `"in-progress"`.

**Step 6 — Complete the skill.**
Resolve the ACT-ID: read `architectures/<arch-id>/<offerfile>.md` frontmatter to get
`activity_id`. This is `<act-id>`.
After the implementation report is written and the required session state is persisted
successfully, update `session.json`: set the `codify` phase `status` to `"approved"`,
set `approved_at` to now, and set `active_phase` to `"validate"`. Display:
> ✓ Implementation report written to `architectures/<arch-id>/<codifyfile>`.
> Next:
```text
/idea.validate <act-id>
```
> to run the governance review.
