---
type: short-term-learning
topic: terraform
created: 2026-05-28 12:30:56
source: codex-conversation
confidence: high
---

# Learning: Binding Resources Clean Up Before Securables

## Context
During the Databricks Unity Catalog workspace-binding refactor, the user pointed out an important lifecycle effect of modeling workspace bindings as separate Terraform resources.

## Learning
When a securable has explicit workspace binding resources that depend on it, Terraform creation and deletion happen in reverse order. On create, Terraform creates the securable first and then creates the binding resources. On destroy, Terraform removes the binding resources first and then deletes the securable. This is useful for cleanup because workspace permissions are removed before the underlying catalog, external location, or storage credential is deleted.

## Future Use
When reviewing or designing Databricks Unity Catalog modules, treat separate "databricks_workspace_binding" resources as a lifecycle advantage, not only extra resource count. They make deletion safer by removing explicit workspace access before deleting the securable.
