# Task Proposals from Codebase Review

## 1) Typo Fix Task
**Issue found:** `candidates.txt` mixes mostly English service names (`beheer`, `api`, `cloud`) with `cloudbeheer` as a concatenated token, which is likely an accidental naming typo/inconsistency in candidate host labeling and can hurt operator readability during recon triage.

**Proposed task:** Normalize candidate hostname naming in `candidates.txt` (e.g., decide whether `cloudbeheer` should be `cloud-beheer` or another standardized form) and document the naming convention.

**Why this matters:** Consistent naming reduces false assumptions during manual review and downstream parsing/reporting.

---

## 2) Bug Fix Task
**Issue found:** `app.js` is a bundled/minified AngularJS app pinned to **AngularJS v1.3.20** (from 2014), which is end-of-life and carries known security and compatibility risk.

**Proposed task:** Upgrade or replace the legacy AngularJS runtime in `app.js` with a supported front-end stack (or at minimum bump to the latest available AngularJS LTS-compatible fork and patch known vulnerabilities).

**Why this matters:** This is a functional/security bug risk in modern environments (old framework behavior, browser regressions, and unpatched security issues).

---

## 3) Code Comment / Documentation Discrepancy Task
**Issue found:** The repository has operational target lists (`targets/*.txt`, `candidates.txt`) but no accompanying documentation explaining file semantics (source, freshness, dedupe policy, and expected format).

**Proposed task:** Add `README.md` documentation that defines:
- what each target file represents,
- required line format (IP vs hostname),
- deduplication/sorting expectations,
- how and when lists should be updated.

**Why this matters:** Current repo contents imply curated recon inputs, but there is no written contract for maintainers/automation.

---

## 4) Test Improvement Task
**Issue found:** There are no tests validating the target and candidate list quality.

**Proposed task:** Add a lightweight test suite (e.g., Node or shell-based CI checks) to validate:
- valid IPv4 formatting in `targets/*.txt`,
- valid hostname formatting in `candidates.txt`,
- no duplicates within a file,
- optional deterministic ordering checks.

**Why this matters:** Prevents silent data-quality regressions in recon inputs.
