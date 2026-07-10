---
type: short-term-learning
topic: terragrunt
created: 2026-07-02 10:50:07
source: codex-conversation
confidence: high
---

# Learning: platform E2E EOFs map to dependency-output consumers

## Context
While analyzing GitLab job 35846961 for `data-platform-terragrunt-catalog`, the second root `terragrunt stack run apply` failed after both `dbx_workspace` units completed and after their 300-second apply hooks.

## Learning
The 9-unit accounting shows five successful units, one early-exited `dbx_compute_policy`, and three bare `EOF` failures. The failed units are inferred as `dbx_workspace_assignments`, `dbx_sandbox_catalogs`, and `dbx_external_locations`: the three units unblocked after workspace/metastore outputs and needing Terragrunt dependency output reads. The log has no guard-script error codes and no Terraform/provider output from those failed units before EOF, so the current trace points above the shell guards, likely at Terragrunt dependency-output resolution or stack-run concurrency.

## Future Use
For similar EOFs, first map successes/failures against the stack DAG. Treat a named early exit as dependency fallout unless that unit emitted provider or guard output. Use `TG_PARALLELISM=1`, higher `TG_LOG_LEVEL`, and per-unit summaries to isolate Terragrunt output-read concurrency before changing feature YAML or guard logic.
