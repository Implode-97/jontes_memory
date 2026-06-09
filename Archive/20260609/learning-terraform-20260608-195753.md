---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:57:53
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper variables contract score

## Context
During the sandbox catalog wrapper scoring sequence, `aws-databricks-workspace-catalog-wrapper-sandbox/variables.tf` was reviewed for readability, KISS, YAGNI, maintainability, and Databricks/Terragrunt lifecycle safety.

## Learning
The variable file is shippable and safety-focused, around the high 70s/low 80s. Its justified complexity is the explicit workspace-provider binding invariant, lifecycle tombstone support, guarded empty-list escape hatch, and mandatory active-row metadata for admin, cost center, and unit name. Small readability debt remains in fake partition flexibility for `permissions_boundary_arn`, repeated/trimmed principal comparisons, a missing `nullable = false` on `skip_validation`, and minor scan friction around the `allow_empty_sandbox_catalog_definitions` flag being referenced before it is declared.

## Future Use
For future reviews, do not recommend removing lifecycle or workspace-binding guards as overengineering. Prefer narrow cleanup that tightens local contracts and removes small unnecessary validation noise.
