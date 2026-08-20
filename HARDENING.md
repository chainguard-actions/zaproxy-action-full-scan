<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-full-scan/v0.12.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-full-scan/v0.12.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tagged version is updated or compromised. Affected references: check-dist.yml — actions/checkout@v4, actions/setup-node@v4, actions/upload-artifact@v4; check-run.yml — actions/checkout@v4.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/check-run.yml:15`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level 'permissions:' key, and no job within either file defines job-level permissions. Without explicit permissions, workflows inherit the default repository token permissions (which may be write-all depending on repository settings), granting broader access than necessary.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-character SHA hashes with original tags preserved as comments — actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02; (2) Added top-level `permissions: {}` to both check-dist.yml and check-run.yml to enforce least-privilege token access.

