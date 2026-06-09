---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:27:06
source: codex-conversation
confidence: high
---

# Learning: Keep group membership outputs contract-shaped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/outputs.tf`, the user asked for a strict KISS/YAGNI Terraform output API lens.

## Learning
For the Databricks account group membership module, `group_id` is the strongest downstream contract because it exposes the resolved target group SCIM ID. Resolved member SCIM ID maps such as `member_group_ids`, `user_member_ids`, and `service_principal_member_ids` are defensible when callers or wrapper tests need canonical identity state, but they still widen the module API if unused. `membership_resource_ids` is the weakest output because it exposes `databricks_group_member` resource IDs and the internal users/groups/service-principals resource split; current usage is test-only.

## Future Use
When reviewing or editing this module, preserve useful resolved identity contracts but prefer direct test assertions over public resource-ID outputs. Remove or mark diagnostic outputs before the module API freezes unless a real downstream caller needs them.
