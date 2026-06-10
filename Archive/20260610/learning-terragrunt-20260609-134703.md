---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:47:03
source: codex-conversation
confidence: high
---

# Learning: Catalog AWS Provider Include Clarity

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/provider_aws.hcl` for looks, readability, KISS, YAGNI, and maintainability.

## Learning
In this Terragrunt catalog, active AWS-using units currently include `provider_aws_assume_role.hcl`, while the simple `provider_aws.hcl` exists but has no catalog consumers. Even though the simple provider file is mechanically small and copied from live conventions, its inactive status creates readability debt because a new platform maintainer cannot tell from the file itself when to choose it over the assume-role variant.

## Future Use
For future reviews, treat unused provider parent configs as acceptable only when the intended consumer or selection rule is obvious from nearby includes or docs. Prefer removing inactive placeholders before go-live, or adding a tight comment/doc line that names the intended use case and why active units should keep using the assume-role include.
