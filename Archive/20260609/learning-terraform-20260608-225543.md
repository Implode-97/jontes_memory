---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:55:43
source: codex-conversation
confidence: high
---

# Learning: Databricks account group naming validation belongs near wrappers

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group/variables.tf`, the user asked for a strict KISS/YAGNI Terraform variable review focused on dense regex, duplicated naming policy, and future-proof enum values.

## Learning
For the `databricks-account-group` leaf module, preserve real display-name safety such as non-null input, simple non-empty/no-whitespace hygiene, and max length. Treat broad canonical naming regexes as maintenance debt when wrappers already derive group names like `grp_team_${team_key}_${role}`, `grp_ws_${region_code}_${workspace_target_name}_${entitlement_group}`, and legacy metastore owner groups.

## Future Use
In future reviews or edits, prefer moving exact role/profile policy to the deriving wrappers or their tests. If the leaf must guard names before API freeze, keep the accepted patterns readable and limited to currently generated values rather than adding future roles or broad workspace profiles.
