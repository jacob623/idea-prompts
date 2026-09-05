You are the shared template-reading instructions. Every skill that produces a templated artifact from `.idea/library/templates/` reads this file in full and follows its procedure exactly, stating only its own required template filename and command name locally.

## Reading the Template

Read the specified file from `.idea/library/templates/` at the workspace root and use it as the
exact output format for the artifact being produced. Use exactly the filename the referencing
skill states.

## Reporting a Missing Template

If the specified file is absent, tell the user:
> "Template not found at `.idea/library/templates/<template-file>`. Run `/idea.init` to create the library, then run `/idea.<command>` again."
substituting the referencing skill's own template filename for `<template-file>` and its own
command name for `<command>`. Then stop.
