<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-full-scan/v0.10.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-full-scan/v0.10.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag refs instead of full 40-character SHA commit hashes, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v3` (check-dist.yml); `actions/checkout@v4` (check-run.yml).

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/check-run.yml:18`

### missing-permissions (severity: medium)

Neither `check-dist.yml` nor `check-run.yml` has a top-level `permissions:` key, and neither job within these files defines a job-level `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all unpinned action references to full 40-character SHA hashes with original tags preserved as comments — actions/checkout@v4→@11d5960a..., actions/setup-node@v4→@49933ea5..., actions/upload-artifact@v3→@ff15f030... in check-dist.yml, and actions/checkout@v4→@11d5960a... in check-run.yml. (2) Added top-level `permissions: contents: read` to both check-dist.yml and check-run.yml to enforce least-privilege access for the GITHUB_TOKEN.

