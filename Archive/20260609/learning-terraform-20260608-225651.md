---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:56:51
source: codex-conversation
confidence: high
---

# Learning: Keep Central Naming Validation Aligned With Generators

## Context
During file-by-file scoring of Databricks account group modules, central group-name validation in a leaf module received lower scores because one dense regex tried to encode multiple wrapper-generated naming schemes.

## Learning
Central naming validation can be useful as a final guardrail for shared identity modules, but it should remain readable and aligned with the modules that generate those names. A single large regex that embeds legacy patterns, canonical team roles, workspace entitlement profiles, and length limits becomes hard to audit and can drift ahead of current wrapper contracts. If extra enum values are intentionally allowed for direct callers or future features, that intent should be documented and covered by focused mock tests.

## Future Use
When writing or reviewing Terraform naming policy, prefer separate readable validations for length, naming families, and allowed enum lists. Check that allowed roles/profiles match current generators or are explicitly justified, and add mock invalid coverage for bad enum values and boundary lengths.
