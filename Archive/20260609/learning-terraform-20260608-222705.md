---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:27:05
source: codex-conversation
confidence: high
---

# Learning: Keep account group membership outputs useful

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/outputs.tf`, the user framed the review as a Databricks account identity consumer check for callers composing group membership modules.

## Learning
The output contract is intentionally useful: it exposes the target group SCIM ID/name, resolved member SCIM IDs by principal type, and `databricks_group_member` resource IDs grouped by principal type. The `membership_resource_ids.groups` and `member_group_ids` outputs are consumed by team identity wrapper tests to verify role nesting, and real provider tests check membership resource IDs are populated. Treat these outputs as shippable contract surface rather than noise.

## Future Use
For later reviews, do not suggest removing `membership_resource_ids` or collapsing the principal-typed output shape unless consumers change. Prefer only tiny cleanup suggestions, such as clarifying descriptions if import/debug use becomes more prominent.
