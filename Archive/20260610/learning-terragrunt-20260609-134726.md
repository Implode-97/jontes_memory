---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:47:26
source: codex-conversation
confidence: high
---

# Learning: Avoid Duplicate AWS Provider Parents Without Current Callers

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/provider_aws.hcl`, a simple generated AWS provider parent beside `provider_aws_assume_role.hcl`.

## Learning
In this Terragrunt catalog, a standalone non-assume-role AWS provider parent is only justified when current units actually need a no-role AWS provider. Current workspace and sandbox catalog units use the assume-role parent, which can represent both cases if `aws_role_arn` is optional or null. Keeping a separate unused parent adds a second generated `provider_aws.tf` owner and increases accidental-include risk, especially because account-scope `_region.hcl` is a compatibility shim rather than a real regional deployment target.

## Future Use
For future provider-generation reviews, prefer one obvious AWS provider parent unless active unit contracts require separate provider modes. Penalize unused parallel provider parents as YAGNI even when the file itself is small and idiomatic.
