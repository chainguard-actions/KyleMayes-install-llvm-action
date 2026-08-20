<!-- markdownlint-disable -->

# Hardening Report: KyleMayes--install-llvm-action/v2.0.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KyleMayes--install-llvm-action/v2.0.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit digests, making them vulnerable to supply-chain attacks if the tag is moved.

- .github/workflows/cache.yml: `actions/checkout@v4`, `actions/cache@v4`, `KyleMayes/install-llvm-action@v2`
- .github/workflows/debug.yml: `actions/checkout@v4`, `mxschmitt/action-tmate@v3`
- .github/workflows/test.yml: `actions/checkout@v4`

All of these should be pinned to their full SHA digest (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/cache.yml:20`
- `.github/workflows/cache.yml:24`
- `.github/workflows/cache.yml:33`
- `.github/workflows/debug.yml:16`
- `.github/workflows/debug.yml:23`
- `.github/workflows/test.yml:23`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no job within them defines job-level `permissions:` either. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (which may be `write-all` for older repositories), granting broader access than necessary. Each workflow should declare minimal required permissions (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/cache.yml:1`
- `.github/workflows/debug.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** — Pinned all 6 mutable tag references to full SHA digests:
- `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4` (in all 3 files)
- `actions/cache@v4` → `@0057852bfaa89a56745cba8c7296529d2fc39830 # v4` (cache.yml)
- `KyleMayes/install-llvm-action@v2` → `@ebc0426251bc40c7cd31162802432c68818ab8f0 # v2` (cache.yml)
- `mxschmitt/action-tmate@v3` → `@35b54afac29c97fb54faba5b513f8fbd1882f113 # v3` (debug.yml)

**missing-permissions** — Added `permissions: contents: read` top-level block to all 3 workflow files (cache.yml, debug.yml, test.yml). This is the minimal permission required for workflows that only check out code.

