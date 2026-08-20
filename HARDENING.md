<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-full-scan/v0.11.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-full-scan/v0.11.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files are pinned to mutable version tags instead of immutable 40-character SHA digests. This exposes the action to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v4` (check-dist.yml line 21, check-run.yml line 14), `actions/setup-node@v4` (check-dist.yml line 24), `actions/upload-artifact@v3` (check-dist.yml line 43).

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/check-run.yml:14`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/check-run.yml` declares a top-level `permissions:` key, and no job within either file declares job-level permissions. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. `write` on `contents` and other scopes). Each workflow should declare minimal required permissions at the top level or per job.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. **unpinned-uses**: Pinned all `uses:` references to full 40-character SHA digests:
   - `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4` (both files)
   - `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4` (check-dist.yml)
   - `actions/upload-artifact@v3` → `actions/upload-artifact@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3` (check-dist.yml)

2. **missing-permissions**: Added `permissions: contents: read` at the top level of both `check-dist.yml` and `check-run.yml`. This is the minimum permission needed for checkout; no write permissions are required by either workflow.

