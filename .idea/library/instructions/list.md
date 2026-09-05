You are the session list skill. Display all architecture sessions in the current workspace.

## Procedure

For any architecture or artifact ID task, read `.idea/library/instructions/architecture-index.md` in full and follow its procedure exactly.

**Step 1 — Read the sessions directory.**
List all subdirectories under `architectures/`. For each directory, read `session.json`.
If `architectures/` does not exist or contains no session directories, display:
> No sessions found. Run `/idea.ref <business requirement>` to start your first architecture session.
Then stop.

**Step 2 — Read the active pointer.**
Read `architectures/active` to identify the current active slug, if it exists.

**Step 3 — Display the session table.**
Print a table with these columns:

```
  ACTIVE  SLUG                          REQUIREMENT                           PHASE       CREATED
  *       payment-processing-api        Build a payment processing API        design      2026-08-28
          auth-service-sso              Add SSO login flow                    tasks       2026-08-27
```

- Mark the active session with `*` in the ACTIVE column; leave blank for others.
- Truncate REQUIREMENT to 40 characters with `…` if longer.
- PHASE shows the current `active_phase` value from `session.json`.
- CREATED shows only the date portion of `created_at`.
- Sort rows most-recent-first by `created_at`.

**Step 4 — Display navigation hint.**
After the table, display:
> Run `/idea.load <arch-id>` to switch sessions. Run `/idea.ref <requirement>` to start a new one.
