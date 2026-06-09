---
type: short-term-learning
topic: databricks
created: 2026-06-09 01:48:45
source: codex-conversation
confidence: high
---

# Learning: Databricks service principal variable naming guard

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-service-principal/variables.tf`, the user asked for a Databricks account identity/platform naming perspective on display-name validation and lifecycle variables.

## Learning
For this one-resource account service principal module, the small variable surface is broadly shippable: `active` and `disable_as_user_deletion` are justified lifecycle controls because deactivate/delete behavior matters for durable account identities. The display-name regex adds useful guardrails against unmanaged account object sprawl, but it is the main cleanup pressure because it encodes `sp_` plus at least three hyphen-separated segments in a leaf module and may conflict with the user's general preference for underscore-style generated IaC-facing identifiers unless account SP display names are a documented exception.

## Future Use
When reviewing or editing this module, preserve the lifecycle booleans. Treat the naming regex as acceptable only if the leaf module intentionally owns platform account-object naming; otherwise prefer moving full convention enforcement to wrappers/registries and keeping leaf validation to basic hygiene plus the Databricks length limit.
