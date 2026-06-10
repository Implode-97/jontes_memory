---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:28:25
source: codex-conversation
confidence: high
---

# Learning: One-Unit Explicit Stacks Can Be Catalog-Clarity

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/eu-central-1/ws_dev/terragrunt.stack.hcl`, a one-unit explicit workspace stack mirrored by sibling catalog and live stack files.

## Learning
In this Terragrunt catalog, a one-unit explicit `terragrunt.stack.hcl` can be justified when it preserves catalog/live symmetry, keeps generated unit paths visible, and maintains focused child stack entrypoints for future unit composition. The active `unit` block with explicit `source`, `path`, and empty `values = {}` is low-cost convention rather than automatic YAGNI. The main readability debt is commented inactive unit placeholders that look like committed future scope.

## Future Use
When scoring small catalog stack files, do not penalize an explicit one-unit stack solely for being one unit. Penalize placeholder commented units unless they are clearly documented as intentionally inactive or immediately useful.
