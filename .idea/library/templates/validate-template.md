---
validate_id: <VLDXXXXXX>
architecture_id: <arch-id>
codify_id: <CDYXXXXXX>
status: pass | fail | conditional-pass
reviewed_at: <UTC timestamp>
---

# Governance Review: <offering title>

**Source artifacts**: `ref.md`, `offer.md`, `tasks.md`, `codify.md`
**Standards**: `.idea/library/standards/`

## Review Summary
- **Critical findings**: [N]
- **Major findings**: [N]
- **Minor findings**: [N]
- **Info findings**: [N]
- **Gate decision**: pass | fail | conditional-pass

---

## Artifact: ref.md

[No findings — or list findings below]

### VLD001
- **Severity**: critical | major | minor | info
- **Standard**: [standard filename, section]
- **Location**: ref.md ## [section name]
- **Description**: [what was found]
- **Remediation**: [what to fix]
- **Return to**: ref phase

---

## Artifact: offer.md

[No findings — or list findings below]

### VLDNNN
- **Severity**: critical | major | minor | info
- **Standard**: [standard filename, section]
- **Location**: offer.md ## [section name]
- **Description**: [what was found]
- **Remediation**: [what to fix]
- **Return to**: offer phase

---

## Artifact: tasks.md

[No findings — or list findings below]

### VLDNNN
- **Severity**: critical | major | minor | info
- **Standard**: [standard filename, section]
- **Location**: tasks.md [Epic / Story / Task]
- **Description**: [what was found]
- **Remediation**: [what to fix]
- **Return to**: tasks phase

---

## Artifact: codify.md

[No findings — or list findings below]

### VLDNNN
- **Severity**: critical | major | minor | info
- **Standard**: [standard filename, section]
- **Location**: codify.md ## [Epic] / [artifact_path]
- **Description**: [what was found]
- **Remediation**: [what to fix]
- **Return to**: codify phase

---

## Gate Decision

**Decision**: pass | fail | conditional-pass

**If fail**: Address the following critical findings before advancing:
- [VLDNNN] — return to [phase]

**If conditional-pass**: The following major findings have been accepted with rationale:
- [VLDNNN] — [architect's rationale for accepting the risk]

**If pass**: All findings are minor or info. Session is complete.

## Traceability
- architecture_id: <arch-id>
- codify_id: <CDYXXXXXX>
