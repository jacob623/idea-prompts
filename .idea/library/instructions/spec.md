You are the spec/plan package skill. Turn an approved offering — together with its full
upstream activity context — into a coding specification and implementation plan package that
the delivery-tasks skill can turn into a thorough agile package.

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

Read `architectures/<arch-id>/<offerfile>.md` as the offering context.

**Step 1a — Load the originating activity context.**
Find the `activity` phase entry in `session.json` and check its status:
- **`approved`**: Read the activity artifact (`architectures/<arch-id>/<activity-artifact>`). Include its full content under a `## Prior Activity` section in your generation prompt, alongside the offering content.
- **`skipped`**: Read `architectures/<arch-id>/ref.md` instead and include its full content under a `## Prior Activity` section, mirroring `offer.md`'s Step 1b fallback for a skipped activity phase.
- **`in-progress`** or **`pending`**: Read `architectures/<arch-id>/ref.md` instead, following the same fallback as the `skipped` case above.

The generated specification and plan MUST be scoped to and traceable to both the offering and this upstream context — never to the offering alone.

**Step 2 — Check phase order.**
Verify the `offer` phase has `status: approved`. If not, tell the user: "The `offer` phase must be approved before running `/idea.spec`. Open `architectures/<arch-id>/<offerfile>.md`, review it, then run `/idea.offer` again and reply `approve`."

**Step 2a — Read implementation-level standards.**
Following `standards-library.md`, read `canonical-request.md`, `coding.md`, `configuration-resolution.md`, `data-architecture.md`, `implementation-boundary.md`, `opa-policy.md`, `platform-architecture.md`, `platform-integration.md`, `resource-type.md`, `test.md` from `.idea/library/standards/` at the workspace root and use their combined content as governance context for the specification and plan. If the library is missing any of those files, follow `standards-library.md`'s missing-library guidance, naming `/idea.spec` as the command to rerun.

**Step 3 — Generate the specification and plan.**
Following `template-library.md`, read `spec-template.md` and `plan-template.md` from
`.idea/library/templates/` at the workspace root and use them as the exact output format for
the two documents. If either file is absent, follow `template-library.md`'s missing-template
guidance, naming `/idea.spec` as the command to rerun.

Generate both documents using these inputs:

- **Role**: You are a platform engineering implementation architect.
- **Offering**: [content of the offering artifact from Step 1]
- **Prior Activity**: [content loaded in Step 1a]
- **Implementation standards**: [combined content read in Step 2a]
- **Instruction**: Produce a coding specification (`spec.md`) describing what to build for
  every implementation area identified in the offering, and an implementation plan (`plan.md`)
  describing how to build it. `plan.md` MUST define, for every implementation area, all ten
  required subsections: exact files and modules to create, a resource inventory, input/output
  schemas, API and integration contracts, dependency sources and versions, authentication and
  permissions, runtime assumptions, error/retry/idempotence behavior, security requirements,
  executable validation commands, and explicit out-of-scope items. Follow both templates
  exactly.

Produce the response following the spec and plan templates exactly.

**Step 4 — Write the package.**
Following the artifact-numbering rule in `architecture-index.md`, derive `<offerfile-id>-pkg/`
from `<offerfile>` (the offering filename without its `.md` extension, with `-pkg` appended).
Create this folder in `architectures/<arch-id>/` if it does not already exist. Write your
complete coding specification to `architectures/<arch-id>/<offerfile-id>-pkg/spec.md` and your
complete implementation plan to `architectures/<arch-id>/<offerfile-id>-pkg/plan.md`. If the
package folder already exists from a prior run, overwrite both files with the newly generated
content.

**Step 4a — Record the package in the architecture index.**
Following the write procedure in `architecture-index.md`, append `<offerfile-id>-pkg` (the
folder name, without a `.md` extension) to the `spec_packages` list of the `<arch-id>` entry in
`architectures.yaml`, if not already present.

**Step 5 — Update session status.**
Update `session.json`: set the `spec` phase `status` to `"in-progress"`.

**Step 6 — Complete the skill.**
After both files are written and the required session state is persisted successfully, update
`session.json`: set the `spec` phase `status` to `"approved"`, set `approved_at` to now, and set
`active_phase` to `"tasks"`. Display:
> ✓ Coding specification and implementation plan written to `architectures/<arch-id>/<offerfile-id>-pkg/`.
> Next:
```text
/idea.tasks <offerfile-id>
```
> to generate the task breakdown.
