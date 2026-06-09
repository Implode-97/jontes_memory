---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:31:41
source: codex-conversation
confidence: high
---

# Learning: Workspace assignments variables are mostly justified

## Context
During a read-only KISS/YAGNI review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/variables.tf`, the user asked for a strict Terraform variable-contract score without tests or other-agent conclusions.

## Learning
For `databricks-workspace-assignments-wrapper`, keep `region_code`, `workspace_target_name`, and positive non-null `workspace_id` as explicit required inputs because they are real naming/workspace contract values, not fake portability. The naming regexes are acceptable because group names are derived directly from these tokens. `team_keys = []` is defensible when no team nesting is a valid use case, but it is the only optional-looking part of the contract and should remain simple.

## Future Use
When reviewing this wrapper, preserve concise shape validation and useful caller diagnostics. Prefer small cleanup over rework: remove redundant `distinct` from list-to-set normalization if touched, consider `set(string)` only if callers do not need list semantics, and avoid adding extra validation beyond module-owned naming/token safety.
