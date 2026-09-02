<!-- markdownlint-disable -->

# Hardening Report: benc-uk--workflow-dispatch/v1.3.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **benc-uk--workflow-dispatch/v1.3.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): ${{ }} expressions are interpolated directly inside run: shell command strings. In echo-1.yaml (line 16), `run: echo '${{ inputs.message }}'` directly embeds the user-controlled workflow_dispatch input into the shell command. In echo-2.yaml (line 18), `echo '${{ inputs.message }}'` does the same. In build-test.yaml, two run: blocks embed `${{ steps.dispatch-fail.outcome }}` and `${{ steps.bad-workflow.outcome }}` directly in shell if-conditions. All of these allow an attacker to inject shell metacharacters via the expression value before the shell ever sees it.

Locations:

- `.github/workflows/echo-1.yaml:16`
- `.github/workflows/echo-2.yaml:18`
- `.github/workflows/build-test.yaml:163`
- `.github/workflows/build-test.yaml:191`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no individual job within any workflow defines a job-level `permissions:` block. This means all jobs run with the default (potentially broad) GITHUB_TOKEN permissions. Affected files: build-test.yaml, echo-1.yaml, echo-2.yaml, echo-3.yaml, test.yaml.

Locations:

- `.github/workflows/build-test.yaml:1`
- `.github/workflows/echo-1.yaml:1`
- `.github/workflows/echo-2.yaml:1`
- `.github/workflows/echo-3.yaml:1`
- `.github/workflows/test.yaml:1`

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable version tags instead of immutable 40-character commit SHA digests, making the workflows vulnerable to supply-chain attacks if the upstream action tag is moved. Failing references: `actions/checkout@v6` (appears 11 times in build-test.yaml and once in test.yaml) and `actions/setup-node@v6` (appears once in build-test.yaml).

Locations:

- `.github/workflows/build-test.yaml:31`
- `.github/workflows/build-test.yaml:32`
- `.github/workflows/test.yaml:8`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions, unpinned-uses

**Notes:**

Fixed all three findings across five workflow files:

1. script-injection: Moved ${{ inputs.message }} in echo-1.yaml and echo-2.yaml into env: blocks (MESSAGE env var). Moved ${{ steps.dispatch-fail.outcome }} and ${{ steps.bad-workflow.outcome }} in build-test.yaml into env: blocks (DISPATCH_FAIL_OUTCOME and BAD_WORKFLOW_OUTCOME respectively).

2. missing-permissions: Added `permissions: {}` top-level block to all five workflow files (build-test.yaml, echo-1.yaml, echo-2.yaml, echo-3.yaml, test.yaml).

3. unpinned-uses: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6 (11 occurrences in build-test.yaml, 1 in test.yaml) and actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6 (1 occurrence in build-test.yaml).

