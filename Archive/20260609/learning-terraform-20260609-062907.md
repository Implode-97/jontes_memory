---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:29:07
source: codex-conversation
confidence: high
---

# Learning: Workspace catalog inputs should explain provider workspace anchors

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-catalog/variables.tf`, reviewers evaluated the module's input API against Databricks workspace-catalog binding behavior and local KISS/YAGNI style.

## Learning
For workspace catalog modules, `workspace_ids` and `provider_workspace_id` are justified even though they create a cross-input contract. Non-empty `workspace_ids` means selected-workspace mode, and `provider_workspace_id` is needed because Databricks automatically binds the creating/provider workspace while the provider does not expose its workspace ID. The input description must make that invariant visible; otherwise the required provider workspace ID looks arbitrary, especially when `workspace_ids = []`.

## Future Use
When reviewing or writing similar variables, keep the explicit selected/open contract, but document that selected bindings are read-write and that the provider workspace must match the configured provider and be included in non-empty `workspace_ids`. Reject or consistently normalize padded identity strings, and make validation messages match the actual guard instead of promising full URL or provider validation.
