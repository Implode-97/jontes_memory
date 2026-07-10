---
type: short-term-learning
topic: terragrunt
created: 2026-07-03 12:49:39
source: codex-conversation
confidence: high
---

# Learning: implicit run-all reproduces repeated E2E EOF

## Context
During `data-platform-terragrunt-catalog` platform E2E debugging, commit `78d3d0a` changed the final diagnostic no-op from explicit `terragrunt stack run apply` to implicit `terragrunt run --all -- apply`.

## Learning
GitLab job `35941079` reproduced the same EOF pattern under Terragrunt `0.99.4`: the first stack apply succeeded, workspace outputs were valid, `run --all` generated the stack and used concurrency 1, then `dbx_external_locations`, `dbx_sandbox_catalogs`, and `dbx_workspace_assignments` failed after dependency output/cache/generate handling but before final-noop hooks or Terraform execution. This rules out the explicit `stack run apply` wrapper and the external-location guard scripts as the main variable for that failure.

## Future Use
Do not keep probing stack-vs-run command syntax for this symptom. Prefer testing a newer Terragrunt version or reducing to a single generated-unit repro after the first successful apply.
