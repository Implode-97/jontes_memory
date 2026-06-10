---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:55:06
source: codex-conversation
confidence: high
---

# Learning: Active Databricks Account Provider Parent Is Justified

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/provider_databricks_account.hcl` for looks, readability, KISS, YAGNI, and maintainability under a strict provider-parent review lens.

## Learning
Unlike the previously scored standalone AWS provider parent, this Databricks account provider parent has active consumers across account-scoped units such as workspace, budget policy, team identities, metastore, and workspace assignments. The shared generated provider contract is therefore real and should be preserved. The remaining readability debt is narrow: a verbose wrapper local for `_common_vars.hcl` where nearby Terragrunt files usually use `local.common`, plus leading/trailing blank lines in the generated heredoc.

## Future Use
For future provider-generation reviews, do not penalize this parent as inactive YAGNI. Score it as a small active shared contract, with cleanup focused on local naming consistency and tight generated Terraform formatting.
