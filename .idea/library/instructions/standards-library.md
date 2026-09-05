You are the shared standards-reading instructions. Every skill that reads governance standards from `.idea/library/standards/` reads this file in full and follows its procedure exactly, stating only its own required filenames and command name locally.

## Reading Standards

Read the specified files from `.idea/library/standards/` at the workspace root and combine
their content as governance/compliance context for the current step. Use exactly the filenames
the referencing skill states; do not add or omit files.

## Reporting a Missing or Incomplete Library

If `.idea/library/standards/` does not exist, or does not contain all of the referencing
skill's required files, tell the user:
> "Standards library not found at `.idea/library/standards/`. Run `/idea.init` to create the standards library, then run `/idea.<command>` again."
substituting the referencing skill's own command name for `<command>`. Then stop.
