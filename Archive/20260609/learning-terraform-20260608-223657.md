---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:36:57
source: codex-conversation
confidence: high
---

# Learning: Keep Databricks Membership Variable Guards Lookup-Focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/variables.tf`, the user asked for a Terraform and Databricks semantics lens on variable types, validation, descriptions, and cross-input contracts.

## Learning
For the Databricks account group membership module, `set(string)` inputs with `nullable = false` and empty defaults are idiomatic because callers can provide any one member type while `main.tf` enforces the cross-input non-empty contract. Preserve non-empty and whitespace hygiene validations, plus the self-nesting precondition in `main.tf`. Treat the `128` character caps on display-name lookups as the main cleanup candidate unless an explicit Databricks API/provider limit is documented, because this module only looks up existing identities and should avoid adding broad naming policy.

## Future Use
In later reviews of this module, score the variable file as shippable but favor narrow cleanup: document or remove the `128` caps, clarify exact/unique lookup semantics for service principal display names, and avoid replacing the explicit typed inputs with a polymorphic abstraction.
