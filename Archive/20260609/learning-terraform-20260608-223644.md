---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:36:44
source: codex-conversation
confidence: high
---

# Learning: Databricks group membership variable contracts

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/variables.tf`, the user asked for a new platform maintainer and safety-aware simplification perspective.

## Learning
For this lookup-only account-level Databricks membership helper, the separate `user_names`, `member_group_display_names`, and `service_principal_display_names` inputs are a reasonable KISS shape because they map directly to different Databricks identity data sources and membership resources. Preserve non-null sets, lookup-string hygiene, at-least-one-member protection, and direct self-nesting prevention. The main readability risk is hidden cross-input contract: empty defaults on all member inputs make each field look optional, while the module actually requires at least one member category and rejects direct self-nesting in `main.tf`.

## Future Use
When reviewing or editing this module, prefer documenting the combined member requirement and exact lookup semantics over restructuring the inputs. Treat max-length validation on lookup names as acceptable only if tied to Databricks/provider behavior or documented consistently; do not add full platform naming regexes to this helper.
