---
type: short-term-learning
topic: terraform
created: 2026-05-27 09:26:37
source: codex-conversation
confidence: high
---

# Learning: Provider Prefix Module Names

## Context
While preparing the sandbox catalog wrapper MR, the user clarified the module naming convention for the data platform Terraform modules repo.

## Learning
Module folder names should be prefixed by the provider context they primarily use. Unity Catalog modules previously named `databricks-uc-*` should use the `databricks-account-*` prefix under the current convention. Wrappers that combine AWS resources with Databricks account-scoped Unity Catalog behavior should use an `aws-databricks-account-*` prefix.

## Future Use
When creating or renaming modules in this repo, choose provider-context prefixes instead of domain-only names. Use `git mv` for directory renames so GitLab MRs show clean renames rather than delete/add churn.
