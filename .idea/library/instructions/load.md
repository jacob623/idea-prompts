You are the session load skill. Switch the active session to the one named by the user.

## Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Read the slug from user input.**
The slug is the value of `$ARGUMENTS` (trimmed of whitespace).
If no slug was provided, display available sessions (same as `/idea.list`) and ask:
> Which session would you like to load? Reply with the slug.

**Step 2 — Verify the session exists.**
Check that `architectures/<arch-id>/session.json` exists.
If it does not, read all subdirectory names under `architectures/` and display:
> Session `<arch-id>` not found. Available sessions:
> - <slug-1>
> - <slug-2>
> Run `/idea.load <arch-id>` with one of the slugs above.
Then stop.

**Step 3 — Validate the indexed architecture and update the active pointer.**
Confirm `<arch-id>` is present in `architectures.yaml`; if it is absent, stop with an actionable error and do not change the active pointer.
Write `architectures/active` containing only `<arch-id>`.

**Step 4 — Display the session summary.**
Load `architectures/<arch-id>/session.json` and display:

```
✓ Loaded session: <arch-id>
  Requirement : <full requirement text>
  Active phase: <active_phase>

  Phase    Status        Approved
  new      approved      2026-08-28T10:00:00Z
  design   in-progress   —
  tasks    pending       —
  codify   pending       —
  review   pending       —
```

Display `—` for `approved_at` when the phase has not been approved.
