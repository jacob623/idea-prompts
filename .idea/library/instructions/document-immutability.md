You are the shared document-immutability instructions. Every skill that produces a research document, Architecture Decision Record, reference architecture, or capability definition reads this file in full and follows its procedure exactly. Do not duplicate this procedure inline in any other instruction file.

## When a Document Is Locked

A research document, Architecture Decision Record (ADR), reference architecture, or capability
definition is locked once its corresponding phase's `status` field in
`architectures/<arch-id>/session.json` is `"approved"`. A document belonging to a phase whose
status is not yet `"approved"` is not locked and remains freely editable as normal.

## What Locking Means

A locked document MUST NOT be edited, overwritten, or replaced for any reason, including
correcting a typo, updating a value, or an explicit request from the user to change it. This
applies regardless of who or what is asking.

## Producing a Change Instead of an Edit

If a genuinely needed change would otherwise modify a locked document, do not touch the locked
file. Instead, follow the artifact-numbering rule in `architecture-index.md` to produce a new,
separately numbered artifact of the same type, recording the requested change there.

## Telling the User

Before producing a new artifact in place of an edit, tell the user:
> "`<file>` is locked because its phase is complete. I'll create a new <artifact type> instead of editing it."
substituting the locked file's path for `<file>` and the referencing skill's own artifact type
(research document, Architecture Decision Record, or reference architecture) for `<artifact type>`.
