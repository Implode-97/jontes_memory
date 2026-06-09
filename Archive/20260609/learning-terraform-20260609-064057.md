---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:40:57
source: codex-conversation
confidence: high
---

# Learning: Keep compute policy rendering separate from output shaping

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-compute-policy/main.tf`, reviewers evaluated the Terraform locals, Databricks cluster policy resources, and policy permission resources.

## Learning
For compute policy modules, the valuable implementation complexity is deterministic 100-character-safe policy naming, JSON decode/merge/encode for module-owned policy rules, hidden fixed `custom_tags.*` injection, and separate `databricks_permissions` with `CAN_USE`. The recurring readability risk is letting intermediate locals grow into output-shaped objects: broad trimming of already-normalized inputs, duplicated name/hash and reserved-key logic across `variables.tf` and `main.tf`, and `policy_assignments` maps that denormalize metadata mostly for outputs rather than resource creation.

## Future Use
When reviewing or writing similar modules, keep policy rendering and permission resources explicit and provider-native. Prefer one source of truth for name collision and reserved-key checks, reject padded stable identifiers instead of silently normalizing when identity matters, and only expose per-principal assignment metadata after confirming a real downstream consumer.
