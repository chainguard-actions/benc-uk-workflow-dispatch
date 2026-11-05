<!-- markdownlint-disable -->

# Hardening Report: benc-uk--workflow-dispatch/v1.3.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **benc-uk--workflow-dispatch/v1.3.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yaml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yaml b/action.yaml
index c767892..66fabc6 100644
--- a/action.yaml
+++ b/action.yaml
@@ -1,6 +1,5 @@
 name: 'Workflow Dispatch'
 description: 'Trigger and chain GitHub Actions workflows with workflow_dispatch events'
-
 inputs:
   workflow:
     description: 'Name, filename or ID of workflow to run'
@@ -31,7 +30,6 @@ inputs:
     description: 'Whether to set the status of this action to failed if the triggered workflow run fails, or is cancelled. Only applies if wait-for-completion is true.'
     required: false
     default: false
-
 outputs:
   runId:
     description: 'The ID of the workflow run that was triggered'
@@ -41,11 +39,10 @@ outputs:
     description: 'The HTML URL of the workflow run that was triggered'
   workflowId:
     description: 'The ID of the workflow that was triggered'
-
 runs:
   using: 'node24'
   main: 'dist/index.js'
-
 branding:
   color: purple
   icon: send

```

Locations:

- `action.yaml`

## Iteration Notes

### Iteration 1

**Notes:**

prior-fix: the earlier action.yaml patch only removed four blank lines and changed no security-relevant content. The current action.yaml is a plain node24 action (main: dist/index.js) with no uses:, run: steps, shell expressions or hardcoded credentials, so there is no security change to re-apply. I left the file as upstream has it.

