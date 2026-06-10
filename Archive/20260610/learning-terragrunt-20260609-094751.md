---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 09:47:51
source: codex-conversation
confidence: high
---

# Learning: Safety Metadata Must Be Enforced Or Clearly Descriptive

## Context
While scoring `data-platform-terragrunt-catalog/example/non-prod-env/_targets.hcl`, reviewers found that fields like `aws_account_id` looked safety-relevant but were only passed through by `_account.hcl` selectors and not consumed by current units.

## Learning
In Terragrunt example and live-style catalogs, metadata that looks like a deployment-safety control should either be actively consumed/enforced or clearly framed as descriptive. Unused account IDs, target names that duplicate map keys, or repeated null role fields can make a simple topology look safer or more generic than it really is.

## Future Use
When reviewing target catalogs, distinguish real wiring inputs from descriptive inventory. Prefer removing unused fields before they become a copied pattern, or add narrow comments only when a field is intentionally reserved for near-term consumers.
