<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-cloudformation-github-deploy/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-cloudformation-github-deploy/v2.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references:
- actions/checkout@v4 (check.yml line 17, package.yml line 14, release.yml line 16)
- actions/setup-node@v4 (check.yml line 19)
- softprops/action-gh-release@v2 (release.yml line 18)
All should be pinned to their full SHA digest, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check.yml:17`
- `.github/workflows/check.yml:19`
- `.github/workflows/package.yml:14`
- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:18`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all mutable action tag references to full commit SHAs across 3 workflow files:
- check.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4; actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4
- package.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
- release.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4; softprops/action-gh-release@v2 → @3bb12739c298aeb8a4eeaf626c5b8d85266b0e65 # v2
All original tags are preserved as inline comments for readability.

