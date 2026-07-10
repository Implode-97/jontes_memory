---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 14:50:07
source: codex-conversation
confidence: high
---

# Learning: Keep Terragrunt guard projections named and typed

## Context
During multi-agent scoring of the CAV-133911 `dbx_external_locations/terragrunt.hcl` unit, reviewers accepted the safety guard pattern but flagged readability risk in dense inline guard JSON projections.

## Learning
For safety-sensitive Terragrunt units that generate JSON for shell/JQ guards, keep the guard mechanism, lifecycle hooks, and unsigned `if_exists = "overwrite"` behavior. Readability should improve by moving dense guard row construction into clearly named locals, reducing repeated path expressions with root locals, and avoiding `try()` patterns that silently collapse malformed nested YAML objects such as `assignments` or `file_events` into empty/default values.

## Future Use
When reviewing or editing similar Terragrunt units, preserve destructive-action protections but prefer named normalized row shapes, explicit provider-workspace naming, and fail-clear operator feedback for malformed registry shape over large inline comprehensions in `generate` blocks.
