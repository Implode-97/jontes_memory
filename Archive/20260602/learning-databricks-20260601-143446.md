---
type: short-term-learning
topic: databricks
created: 2026-06-01 14:34:46
source: codex-conversation
confidence: high
---

# Learning: Sandbox catalog ownership for Terraform cleanup

## Context
During sandbox catalog E2E cleanup debugging, Databricks rejected catalog deletion because the catalog was owned by a team admin group while the Terraform deployer only belonged to the metastore owner/admin path.

## Learning
For Terraform-managed sandbox catalogs, the Databricks catalog owner should be the platform or regional metastore owner group, not the team admin group. Team owner/admin groups should receive catalog grants instead. `ALL_PRIVILEGES` is suitable for broad team catalog operation because Databricks documents that it does not include `MANAGE`, so it does not grant ownership transfer, grant-management, or delete authority. This avoids Terraform destroy ordering issues where grant resources are removed before the catalog, while preserving a cleanup-capable owner.

## Future Use
When updating sandbox catalog modules or Terragrunt units, pass the metastore owner group as the catalog owner and grant the team owner/admin principal `ALL_PRIVILEGES`. Do not rely on metastore-level deployer permissions or a separate deployer grant to make catalog deletion stable.
