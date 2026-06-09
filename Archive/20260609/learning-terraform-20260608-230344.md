---
type: short-term-learning
topic: terraform
created: 2026-06-08 23:03:44
source: codex-conversation
confidence: high
---

# Learning: Metastore wrapper main composition is shippable with narrow comments

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-metastore-wrapper/main.tf`, the user asked for a new platform maintainer / pragmatic ship reviewer lens focused on readability, KISS, YAGNI, maintainability, naming changes, memberships, and metastore creation.

## Learning
The wrapper main file should be treated as good, shippable composition rather than overengineered Terraform. Keep the locals and explicit child module calls: they make the Terragrunt-facing API small while centralizing metastore, owner-group, and service-principal naming. The deployer service-principal lookup by application/client ID is justified for account-scoped Databricks providers. The main maintainability gaps are unstated assumptions: the custom AWS region-to-short-code formula and the membership `depends_on` ordering before metastore creation.

## Future Use
For future reviews or edits, prefer narrow cleanup over rework: add short comments for the region-code convention and Databricks/provider ordering, and consider deriving service-principal and owner-group display names from `local.metastore_name` to reduce duplicated naming fragments. Do not replace this with broader abstraction or a separate region-code input unless requirements change.
