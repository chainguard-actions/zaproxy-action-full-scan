<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-full-scan/v0.13.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-full-scan/v0.13.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tagged version is updated or compromised. Failing references in check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4`. Failing reference in check-run.yml: `actions/checkout@v4`.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/check-run.yml:17`

### missing-permissions (severity: medium)

Neither workflow file defines a `permissions:` block at the top level or at the job level. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Both `check-dist.yml` and `check-run.yml` are missing permissions declarations entirely.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all four unpinned action references to full 40-char SHAs — actions/checkout@11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@ea165f8d65b6e75b540449e92b4886f43607fa02 — with # v4 comments for readability. (2) Added top-level `permissions: contents: read` block to both check-dist.yml and check-run.yml, restricting the GITHUB_TOKEN to the minimum needed.

