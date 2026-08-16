<!-- markdownlint-disable -->

# Hardening Report: benc-uk--workflow-dispatch/v1.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **benc-uk--workflow-dispatch/v1.2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: `${{ inputs.message }}` is directly interpolated inside a `run:` shell command string. GitHub Actions substitutes the expression value before the shell parses the command, allowing an attacker to inject arbitrary shell metacharacters via the `message` workflow_dispatch input. Offending line: `run: echo '${{ inputs.message }}'`

Locations:

- `.github/workflows/echo-1.yaml:16`
- `.github/workflows/echo-2.yaml:16`
- `.github/workflows/echo-3.yaml:20`

### unpinned-uses (severity: high)

One or more `uses:` references are pinned to mutable tags rather than full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references: `actions/checkout@v4` (build-test.yaml) and `actions/checkout@v2` (test.yaml).

Locations:

- `.github/workflows/build-test.yaml:14`
- `.github/workflows/test.yaml:6`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions. The `permissions:` block in build-test.yaml is commented out and therefore inactive.

Locations:

- `.github/workflows/build-test.yaml:1`
- `.github/workflows/echo-1.yaml:1`
- `.github/workflows/echo-2.yaml:1`
- `.github/workflows/echo-3.yaml:1`
- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three finding types across five workflow files:

1. script-injection (echo-1.yaml, echo-2.yaml, echo-3.yaml): Moved `${{ inputs.message }}` from the `run:` shell string into a step-level `env:` block as `MESSAGE`, and changed the shell command to reference `"$MESSAGE"` safely.

2. unpinned-uses (build-test.yaml, test.yaml): Pinned `actions/checkout@v4` to full SHA `11d5960a326750d5838078e36cf38b85af677262` and `actions/checkout@v2` to full SHA `0717577d45739eb3c851188b29f50ed6c0b2194e`, preserving the original tag in a trailing comment.

3. missing-permissions (all five files): Added `permissions: {}` at the workflow top level in all five files. The previously commented-out `permissions: contents: write` in build-test.yaml was replaced with an active `permissions: {}` since no write access is needed by the active steps.

