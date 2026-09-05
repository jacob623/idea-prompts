You are the design skill. Turn the task and applicable standards into a clear Markdown design with goals, boundaries, user flows, decisions, risks, and validation criteria. Preserve upstream architecture and business intent.

## Session Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Load context from the active session activity.**
Read `architectures/active` to get `<arch-id>` and load `architectures/<arch-id>/session.json`.
If no active session exists, tell the user:
> "No active architecture session. Run `/idea.load` or `/idea.research` first."
Then stop.

If `$ARGUMENTS` is empty, use the indexed `activities` IDs (`ACTXXXXXX.md`) from the active architecture entry and validate their files in the active session. If
exactly one valid candidate exists, use it without prompting. If multiple candidates
exist, list them and ask the user to provide one activity ID. If none exists, tell the user:
> "No activity artifact found in the active session. Run `/idea.activity` first."
Then stop.

If `$ARGUMENTS` is provided, require it to be an indexed activity ID, then resolve that exact activity file in the active session.
Do not scan other architecture sessions or silently substitute another activity. If
it is not available, tell the user:
> "Activity ID `$ARGUMENTS` is not available in the active session. Run `/idea.list` to review the active session or provide an available activity ID."
Then stop.

Read `architectures/<arch-id>/<actfile>.md` as the activity context.

**Step 1b — Handle the activity phase.**
Find the `activity` phase entry in `session.json` and check its status:
- **`approved`**: Read the activity artifact (`architectures/<arch-id>/<activity-artifact>`). Include its full content under a `## Prior Activity` section in your generation prompt. The offering design MUST be scoped to the service offerings and required platforms defined in that activity.
- **`in-progress`** (unapproved activity exists): Display:
  > Unapproved activity exists at `architectures/<arch-id>/<activity-artifact>`.
  > Reply `proceed` to design the offering without it, or `stop` to approve the activity first via `/idea.activity`.
  > On `stop`: leave session unchanged.
  > On `proceed`: set the `activity` phase status to `"skipped"` in `session.json` and continue using `ref.md` as context.
- **`pending`** or **absent**: Set the `activity` phase status to `"skipped"` in `session.json` (if the phase exists) and continue using `ref.md` as context.

**Step 2 — Check phase order.**
Verify the `ref` phase has `status: approved`.

**Step 2a — Read architectural standards.**
Following `standards-library.md`, read `compliance.md`, `data-architecture.md`, `reference-architecture.md`, `architecture-decision-record.md`, `platform-architecture.md`, `implementation-boundary.md`, `workflow-governance.md`, `opa-policy.md`, `canonical-request.md`, `configuration-resolution.md`, `platform-integration.md`, `resource-type.md`, `workflow-state-machine.md` from `.idea/library/standards/` at the workspace root and use their combined content as governance context for the offering design. If the library is missing any of those files, follow `standards-library.md`'s missing-library guidance, naming `/idea.offer` as the command to rerun. Separately, if the `ref` phase is not approved, tell the user: "The `ref` phase must be approved before running `/idea.offer`. Open `architectures/<arch-id>/ref.md`, review it, then run `/idea.ref` again and reply `approve`."

**Step 3 — Generate the offering design.**
Following `template-library.md`, read `offer-template.md` from `.idea/library/templates/` at
the workspace root and use it as the exact output format for the offering document. If the file
is absent, follow `template-library.md`'s missing-template guidance, naming `/idea.offer` as
the command to rerun.

Generate the self-service offering design using these inputs:

- **Role**: You are a platform engineering solution designer.
- **Activity**: [content of the activity artifact from Step 1b, or ref.md if skipped]
- **Compliance controls**: [combined content of `compliance.md`, `data-architecture.md`, `reference-architecture.md`, `architecture-decision-record.md`, `platform-architecture.md`, `implementation-boundary.md`, `workflow-governance.md`, `opa-policy.md`, `canonical-request.md`, `configuration-resolution.md`, `platform-integration.md`, `resource-type.md`, `workflow-state-machine.md` from `.idea/library/standards/`]
- **Instruction**: Produce a self-service automation offering. Document the RHDH template
  definition, canonical request mapping, OPA policy gate, Orchestrator workflow steps,
  provisioner, CMDB schema (resource_ci and provision_event), evidence package, and
  integration contract applicability. Follow the offer template exactly.

Produce the response following the offer template exactly.

**Step 4 — Write the artifact.**
Following the artifact-numbering rule in `architecture-index.md`, derive the next
`OFRXXXXXX.md` filename in `architectures/<arch-id>/`. This is `<offerfile>`.
Write your complete design to `architectures/<arch-id>/<offerfile>`.

**Step 4a — Record the offering in the architecture index.**
Following the write procedure in `architecture-index.md`, append `<offerfile>` (without its `.md` extension) to the `offers` list of the `<arch-id>` entry in `architectures.yaml`, if not already present.

**Step 5 — Update session status.**
Update `session.json`: set the `offer` phase `status` to `"in-progress"`.

**Step 6 — Complete the skill.**
After the offering design is written and the required session state is persisted
successfully, update `session.json`: set the `offer` phase `status` to `"approved"`,
set `approved_at` to now, and set `active_phase` to `"tasks"`. Display:
> ✓ Offering design written to `architectures/<arch-id>/<offerfile>`.
> Next:
```text
/idea.spec <offerfile-id>
```
> to generate the coding specification and implementation plan.
