<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-cloudformation-github-deploy/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-cloudformation-github-deploy/v1.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable tag or branch refs instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the referenced action is compromised or its tag is moved.

- check.yml: `actions/checkout@v3` (line 14), `actions/setup-node@v3` (line 16)
- package.yml: `actions/checkout@v3` (line 14)
- release.yml: `actions/checkout@master` (line 13), `actions/create-release@latest` (line 16)

All should be replaced with full 40-character hex commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check.yml:14`
- `.github/workflows/check.yml:16`
- `.github/workflows/package.yml:14`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:16`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` key, and no job within any of them defines job-level `permissions:` either. Without explicit permissions, workflows run with the default (often broad) token permissions, which can allow unintended write access to repository contents, packages, or other resources.

Affected files:
- .github/workflows/check.yml — no permissions block at top level or job level
- .github/workflows/package.yml — no permissions block at top level or job level (this workflow also pushes to master, making least-privilege permissions especially important)
- .github/workflows/release.yml — no permissions block at top level or job level

Locations:

- `.github/workflows/check.yml:1`
- `.github/workflows/package.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references across 3 workflow files by replacing mutable tag/branch refs with full 40-character commit SHAs (resolved via lookup_action_sha): actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, actions/checkout@master → @61b9e3751b92087fd0b06925ba6dd6314e06f089, actions/create-release@latest → @0cb9c9b65d5d1901c1f53e5e66eaf4afd303e70e. Added top-level permissions blocks to all 3 workflow files: check.yml gets `contents: read` (read-only CI), package.yml gets `contents: write` (needs to push dist/ commits to master), release.yml gets `contents: write` (needs to create GitHub releases).

