---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:50:23
source: codex-conversation
confidence: high
---

# Learning: workspace assignment registry guard shape checks

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/registry-guard.sh` for readability, KISS/YAGNI, maintainability, and Databricks workspace assignment safety.

## Learning
The registry guard's core invariant is valid and should be preserved: active workspace placement rows (`enabled` or `destroy_pending`) may only reference team registry rows whose state is `enabled`; destroyed placement rows are ignored. The script is compact and readable, but its top-level `has("workspace_teams") and has("teams")` check is weaker than its message and validate-time role. Because `terragrunt.hcl` runs this registry guard on `validate` without first running `required-input-guard.sh`, malformed array or row shapes can fall through into cryptic `jq` errors.

## Future Use
For future workspace-assignment guard reviews, accept the registry invariant and all-violation reporting, but ask for a minimal self-contained shape preflight or for the required-input guard to run on `validate` before registry checks.
