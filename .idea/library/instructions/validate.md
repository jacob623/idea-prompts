You are the governance review skill. Review all supplied workflow output artifacts against the applicable governance, policy, architecture, boundary, and testing standards. Return read-only, evidence-backed findings with severity and remediation. Do not modify artifacts.

## Session Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Load context from the active session activity.**
Read `architectures/active` to get `<arch-id>` and load `architectures/<arch-id>/session.json`.
If no active session exists, tell the user:
> "No active architecture session. Run `/idea.load` or `/idea.research` first."
Then stop.

If `$ARGUMENTS` is empty, use the indexed `validations` IDs (`ACTXXXXXX.md` activity input and `VLDXXXXXX.md` validation output) from the active architecture entry and validate their files in the active session. If
exactly one valid candidate exists, use it without prompting. If multiple candidates
exist, list them and ask the user to provide one activity ID. If none exists, tell the user:
> "No activity artifact found in the active session. Run `/idea.activity` first."
Then stop.

If `$ARGUMENTS` is provided, require it to be an indexed validation/activity ID, then resolve that exact activity file in the active session.
Do not scan other architecture sessions or silently substitute another activity. If
it is not available, tell the user:
> "Activity ID `$ARGUMENTS` is not available in the active session. Run `/idea.list` to review the active session or provide an available activity ID."
Then stop.

Read `architectures/<arch-id>/<actfile>.md` as the activity artifact.

**Traverse the artifact chain to assemble the full implementation scope:**
1. Find `OFRXXXXXX.md` in `architectures/<arch-id>/` where `activity_id` in frontmatter equals `$ARGUMENTS`.
2. Confirm `architectures/<arch-id>/<OFR-ID>-pkg/tasks.md` exists (the OFR file ID found above).
3. Find `CDYXXXXXX.md` where `offer_id` in frontmatter equals the OFR file ID found above.
If any artifact in the chain is missing, tell the user which artifact is missing and stop.

**Step 2 — Read governance standards.**
**Step 3 — Run the governance review.**
Read all artifacts from the session directory (`ref.md`, `offer.md`, `tasks.md`,
`codify.md`). Evaluate each artifact against the following four categories only —
do not re-evaluate architecture decisions locked by the ADR:

1. **LLM drift from design**: Verify codify implemented what offer.md specified —
   OPA policy conditions match `## Policy Gate`; workflow states match
   `## Workflow Definition`; Terraform variables match `## Provisioner ### HCP Terraform`;
   Flyway migrations match `## CMDB Schema`.
2. **Cross-artifact consistency**: Verify artifacts agree with each other —
   `codify.md` artifact paths match `tasks.md` specified paths; OPA authorized roles
   match `offer.md ## Policy Gate`; `codify.md` Unresolved Items are none or accepted.
3. **Implementation boundary compliance**: Verify codify stayed within scope —
   no capabilities beyond `offer.md` scope; no architecture ownership boundaries
   violated per `platform-architecture.md`; no undocumented external dependencies.
4. **Test and documentation completeness**: Verify all ADS mandatory tasks completed —
   ADS-R3 policy test present and verified pass; ADS-R4 testing and documentation
   tasks present for each artifact; ADS-R5 workflow test present and verified pass.

**Out of scope**: cloud provider/platform selection; availability and RPO/RTO targets;
architecture capability definitions; NFR values. These were locked by the ADR.

Assign each finding a `VLDNNN` ID, severity (critical/major/minor/info), standard
reference, artifact location, description, remediation, and return-to phase.

**Step 4 — Write the review.**
Following `template-library.md`, read `validate-template.md` from `.idea/library/templates/`
at the workspace root and use it as the exact output format for the review document. If the
file is absent, follow `template-library.md`'s missing-template guidance, naming
`/idea.validate` as the command to rerun.

Populate the template from the findings in Step 3. Following the artifact-numbering rule in
`architecture-index.md`, derive the next `VLDXXXXXX.md` filename in
`architectures/<arch-id>/`. This is `<validatefile>`.
Populate the template from the findings in Step 3. Write the completed review to
`architectures/<arch-id>/<validatefile>`.

**Step 4a — Record the validation in the architecture index.**
Following the write procedure in `architecture-index.md`, append `<validatefile>` (without its `.md` extension) to the `validations` list of the `<arch-id>` entry in `architectures.yaml`, if not already present.

**Step 5 — Apply the gate decision.**
Evaluate the findings:
- **No critical findings**: Update `session.json` — set `validate` phase `status` to
  `"approved"`, set `approved_at` to now, set `active_phase` to `"complete"`.
- **Critical findings present**: Do NOT mark the session complete. Set `validate` phase
  `status` to `"in-progress"`. Display the critical findings and direct the architect:
  > "Critical findings require remediation. Return to the [phase] phase and address
  > [VLDNNN list] before re-running `/idea.validate`."

**Step 6 — Display a completion summary.**
If session is complete, display:
> ✓ Governance review complete. Findings written to `architectures/<arch-id>/<validatefile>`.
> Gate decision: [pass | conditional-pass]. Session `<arch-id>` is complete.
> Run `/idea.list` to see all sessions.

If session is in-progress (critical findings), display:
> ✗ Governance review found critical findings. Review `architectures/<arch-id>/validate.md`.
> Address the findings and re-run `/idea.validate` after remediation.
