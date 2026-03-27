# Codebase review: prioritized fix tasks

## 1) Typo fix task
**Issue found:** The target file `targets/airtunes.txt` uses a service name spelling/casing that can be confused with modern terminology (`AirPlay`) and with the typical product-case form (`AirTunes`).

**Task:** Rename `targets/airtunes.txt` to `targets/airplay.txt` (or `targets/air_tunes.txt` if preserving protocol naming), and update any scripts/docs that reference the old filename.

**Acceptance criteria:**
- No references to `targets/airtunes.txt` remain in repo tooling/docs.
- A short migration note is added in docs/changelog.

## 2) Bug fix task
**Issue found:** Several hosts appear in multiple target lists (`targets/ajp.txt` and `targets/webui.txt`), e.g. `10.10.0.120`, `10.10.0.93`, `10.10.1.172`, `10.10.2.148`, etc. If scan orchestration concatenates lists without deduplication, identical hosts are scanned repeatedly.

**Task:** Add a normalization/dedup step before dispatching scans so each `(host, port/service)` is executed once per run.

**Acceptance criteria:**
- Duplicate entries no longer trigger duplicate scan jobs.
- Run output clearly reports de-duplicated counts.

## 3) Comment/documentation discrepancy task
**Issue found:** `app.js` begins with an AngularJS v1.3.20 license header, but the file is a large bundled/minified artifact containing both framework code and app templates. The repo lacks a short maintainer note describing source-of-truth and rebuild/update process.

**Task:** Add/refresh documentation explaining that `app.js` is a generated vendor/app bundle, where source comes from, and the exact regeneration process (toolchain + command).

**Acceptance criteria:**
- A maintainer can regenerate `app.js` from documented steps.
- Documentation clarifies ownership/versioning of embedded third-party code.

## 4) Test improvement task
**Issue found:** No automated checks currently validate target-list quality (IP format, duplicate detection, stable ordering, non-empty files).

**Task:** Add a lightweight test suite (e.g., shell + Python or Node) that validates all `targets/*.txt` files and `candidates.txt`.

**Acceptance criteria:**
- CI/local test command fails on malformed IP/hostname lines.
- CI/local test command fails on unintended duplicates.
- Test output points to file + line of failures.
