---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:24:42
source: codex-conversation
confidence: high
---

# Learning: Preserve explicit Databricks account group membership shape

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/main.tf`, the user asked for a Databricks account identity/domain perspective focused on readability, KISS, YAGNI, maintainability, and safe account group membership behavior.

## Learning
The `main.tf` shape is high quality and should be treated as shippable: it looks up an existing target group, resolves users, nested groups, and service principals through Databricks data sources, and creates typed `databricks_group_member` resources without creating identities, entitlements, or Unity Catalog permissions. The preconditions for non-empty member input and direct self-nesting prevention are justified safety guards. Separate typed resources are clearer than a merged polymorphic membership map, even though they duplicate a few lines.

## Future Use
For later reviews of this module, keep the explicit account SCIM membership boundary and typed resource blocks. Focus cleanup suggestions on provider/account-scope clarity in docs or call sites, and only consider removing ID locals if outputs stop needing those resolved maps. Do not suggest broad role, entitlement, permission, or identity-creation behavior here.
