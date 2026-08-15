<!-- markdownlint-disable -->

# Hardening Report: aws-actions--aws-cloudformation-github-deploy/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **aws-actions--aws-cloudformation-github-deploy/v2.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of pinned full-length SHA digests. This exposes the workflow to supply-chain attacks where a tag can be silently moved to point to malicious code.

.github/workflows/check.yml:
  - uses: actions/checkout@v3  (line 17)
  - uses: actions/setup-node@v3  (line 19)

.github/workflows/package.yml:
  - uses: actions/checkout@v3  (line 17)

.github/workflows/release.yml:
  - uses: actions/checkout@v4  (line 16)
  - uses: softprops/action-gh-release@v2  (line 18)

All of these should be pinned to a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check.yml:17`
- `.github/workflows/check.yml:19`
- `.github/workflows/package.yml:17`
- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:18`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all 5 unpinned action references across 3 workflow files:
- check.yml: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3; actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3
- package.yml: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3
- release.yml: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4; softprops/action-gh-release@v2 → @3bb12739c298aeb8a4eeaf626c5b8d85266b0e65 # v2
All SHAs were resolved using lookup_action_sha and the original tags are preserved as inline comments.

