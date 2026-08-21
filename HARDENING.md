<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-full-scan/v0.9.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-full-scan/v0.9.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tags instead of full 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the tag is moved.

In `.github/workflows/check-dist.yml`:
- `uses: actions/checkout@v4` (line 20)
- `uses: actions/setup-node@v4` (line 23)
- `uses: actions/upload-artifact@v3` (line 40)

In `.github/workflows/check-run.yml`:
- `uses: actions/checkout@v4` (line 14)

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:40`
- `.github/workflows/check-run.yml:14`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/check-run.yml` declares a top-level `permissions:` block, and no job in either file has a job-level `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions. A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or per job.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. `.github/workflows/check-dist.yml`:
   - Added top-level `permissions: contents: read` block
   - Pinned `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4`
   - Pinned `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
   - Pinned `actions/upload-artifact@v3` → `@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3`

2. `.github/workflows/check-run.yml`:
   - Added top-level `permissions: contents: read` block
   - Pinned `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4`

All SHAs were resolved using lookup_action_sha and are real commit SHAs, not invented values.

