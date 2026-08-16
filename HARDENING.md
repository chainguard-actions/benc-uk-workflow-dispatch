<!-- markdownlint-disable -->

# Hardening Report: benc-uk--workflow-dispatch/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **benc-uk--workflow-dispatch/v1.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: ${{ inputs.message }} is directly interpolated inside run: shell command strings. YAML template substitution occurs before the shell processes the string, so even single-quoting does not prevent injection — an attacker can supply a value like `' ; malicious_command ; '` to break out and execute arbitrary commands. Affected steps: echo-1.yaml 'Echo message', echo-2.yaml 'Echo message slowly with a wait', echo-3.yaml 'Echo message'. Fix: move the value into an env: variable and reference it as a quoted shell variable, e.g. `env: { MESSAGE: "${{ inputs.message }}" }` then `run: echo "$MESSAGE"`.

Locations:

- `.github/workflows/echo-1.yaml:16`
- `.github/workflows/echo-2.yaml:17`
- `.github/workflows/echo-3.yaml:17`

### unpinned-uses (severity: high)

actions/checkout@v6 is referenced by a mutable tag rather than a full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, enabling a supply-chain attack. Fix: pin to the full SHA, e.g. `actions/checkout@<40-char-sha> # v6`.

Locations:

- `.github/workflows/build-test.yaml:14`
- `.github/workflows/test.yaml:9`

### missing-permissions (severity: medium)

None of the workflow files declare a top-level permissions: block, and no job in any of these files has a job-level permissions: block. In build-test.yaml the permissions block is commented out. Without explicit permissions, workflows run with the repository's default token permissions (often write-all), violating the principle of least privilege. Fix: add a top-level `permissions: {}` (or specific minimal scopes) to each workflow.

Locations:

- `.github/workflows/build-test.yaml:1`
- `.github/workflows/test.yaml:1`
- `.github/workflows/echo-1.yaml:1`
- `.github/workflows/echo-2.yaml:1`
- `.github/workflows/echo-3.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings across 5 workflow files: (1) script-injection in echo-1.yaml, echo-2.yaml, echo-3.yaml — moved `${{ inputs.message }}` out of run: shell strings into env: blocks as MESSAGE variable, referenced as "$MESSAGE" in shell; (2) unpinned-uses in build-test.yaml and test.yaml — pinned actions/checkout@v6 to full SHA d23441a48e516b6c34aea4fa41551a30e30af803 with # v6 comment; (3) missing-permissions in all 5 workflow files — added top-level `permissions: {}` block (the build-test.yaml had a commented-out write permissions block which was replaced with the empty permissions block since no write access is needed).

