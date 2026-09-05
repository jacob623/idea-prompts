You are the shared architecture-index instructions. Every skill that resolves or allocates an architecture or artifact ID reads this file in full and follows its procedure exactly. Do not duplicate this procedure inline in any other instruction file.

## ID Prefixes

| ID type | Prefix | Format |
|---------|--------|--------|
| Architecture | `ARCH` | `ARCH######` |
| Research | `RES` | `RES######` |
| ADR | `ADR` | `ADR######` |
| Reference architecture | `REF` | `REF######` |
| Capability | `CAP` | `CAP######` |
| Activity | `ACT` | `ACT######` |
| Offering | `OFR` | `OFR######` |
| Code | `CDY` | `CDY######` |
| Validation | `VLD` | `VLD######` |
| Control | `CTRL` | `CTRL######` |
| NFR | `NFR` | `NFR######` |
| Implementation | `IMP` | `IMP######` |

Each `######` is a six-digit, zero-padded, sequential number. An ID never includes a file
extension in `architectures.yaml`; the corresponding artifact file adds `.md`.

## Reading the Index

Before resolving, allocating, or updating any architecture or artifact ID, read
`architectures.yaml` at the repository root. Reread it on every invocation — never reuse a
copy read earlier in the conversation or by an earlier skill.

If `architectures.yaml` does not exist, tell the user:
> "`architectures.yaml` not found at the repository root. Run `idea init` to create it, then run this skill again."
Then stop. Do not create the file from within a workflow skill.

## Allocating a New Architecture ID

To create a new architecture entry, scan the `id` values already present in
`architectures.yaml`. The new ID is `ARCH<highest+1>`, zero-padded to six digits
(`ARCH000001` if the index has no entries).

## Allocating a New Artifact ID

To allocate a new artifact ID of a given type (research, ADR, reference architecture,
capability, activity, offering, code, or validation) within an architecture, scan
`architectures/<arch-id>/` for existing files matching that type's prefix (for example
`RESXXXXXX.md` for research). The next filename is one greater than the highest number found
among those files, zero-padded to six digits. If none exist, use `<PREFIX>000001.md`. The ID
recorded in `architectures.yaml` is that filename without its `.md` extension.

Exception: `CTRL` (control) artifacts scan `architectures/<arch-id>/controls/` instead of the
architecture's top-level directory. The next `CTRLXXXXXX.md` filename is one greater than the
highest number found among existing files in that subfolder, following the same zero-padding
and `<PREFIX>000001.md` fallback rule.

Exception: `NFR` artifacts scan `architectures/<arch-id>/nfrs/` instead of the architecture's
top-level directory. The next `NFRXXXXXX.md` filename is one greater than the highest number
found among existing files in that subfolder, following the same zero-padding and
`<PREFIX>000001.md` fallback rule.

Exception: `IMP` (implementation) artifacts scan `architectures/<arch-id>/implementations/`
instead of the architecture's top-level directory. The next `IMPXXXXXX.md` filename is one
greater than the highest number found among existing files in that subfolder, following the
same zero-padding and `<PREFIX>000001.md` fallback rule.

## Writing an Update

Immediately before writing any change, reread `architectures.yaml` so the update is based on
the current file, not an earlier in-memory copy. Then:

- **New architecture**: append a new entry with the allocated `id` and no other fields set.
- **Singular field** (`research`, `adr`, `ref`): set the field on the matching architecture
  entry to the new ID, replacing any prior value for that field.
- **`requirement_summary`**: set alongside `research`, to a one-sentence summary of the
  business requirement; replace any prior value, never append.
- **`reference_architecture_title`**: set alongside `ref`, to the reference architecture's own
  title; replace any prior value, never append.
- **Plural field** (`capabilities`, `activities`, `offers`, `spec_packages`, `code`,
  `validations`, `controls`, `nfrs`, `implementations`): append the new ID to the matching list only if it is not already present;
  never remove or reorder existing IDs. `spec_packages` entries are package folder names
  (`<offerfile-id>-pkg`) derived from an existing offering ID, not newly allocated IDs from the
  prefix table above.

Write the complete file back, preserving every other entry and field unchanged.

## Reporting Malformed or Duplicate Content

If `architectures.yaml` is empty in a way that is not a valid index, contains a duplicate
architecture ID, contains a duplicate ID within one artifact list, or contains an ID that does
not match the format in the prefix table above, stop and tell the user:
> "`architectures.yaml` has an invalid entry: <specific problem>. Fix the file, then run this skill again."
Do not guess a corrected value or silently drop the invalid entry.

## Updating session.json

Immediately before writing any change to `architectures/<arch-id>/session.json`, reread it so
the update is based on the current file, not an earlier in-memory copy — the same freshness
rule that applies to `architectures.yaml` above.

Every workflow phase entry in `session.json.phases` is created once, in full, when the
architecture session is created (see `research.md`'s Step 1). Updating a phase's `status`,
`artifact`, or `approved_at` MUST modify only that phase's existing entry in the `phases`
array. The write MUST preserve every other `phases` entry, the `nfrs` object, and every other
top-level field unchanged — mirroring the "preserving every other entry and field unchanged"
rule already required for `architectures.yaml` updates. Never replace or drop unrelated phase
entries when updating one phase's state.
