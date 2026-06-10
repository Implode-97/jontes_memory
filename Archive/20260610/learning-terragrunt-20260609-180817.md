---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 18:08:17
source: codex-conversation
confidence: high
---

# Learning: DBCore Account Selectors Should Stay Explicit

## Context
This came from reviewing `data-platform-terragrunt-live/non-prod/eu-central-1/dbcore/_account.hcl` for live account/provider safety, readability, KISS, YAGNI, and maintainability.

## Learning
A regional dbcore `_account.hcl` selector that explicitly reads `_targets.hcl` and selects the matching key, such as `dbcore_euc1`, is the preferred live shape. Passing through a narrow set of locals like `account_name_acronym`, `aws_account_id`, `aws_role_arn`, and `tags` is acceptable duplication when it forms the stable consumer contract for generated units and provider parents.

## Future Use
When reviewing similar live account selectors, reward explicit target keys over path-derived or generic selection logic. Penalize only real drift risks, such as selecting the wrong regional key, leaking workspace-only fields into dbcore, or adding unused alternate selector modes.
