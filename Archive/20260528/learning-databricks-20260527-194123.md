---
type: short-term-learning
topic: databricks
created: 2026-05-27 19:41:23
source: codex-conversation
confidence: high
---

# Learning: Databricks automatic anchor binding affects Terraform destroy

## Context
While fixing the sandbox catalog wrapper real apply CI in the Terraform modules repo, the first code failure was a destroy-time Databricks error: an external location became inaccessible in the current workspace before Terraform could delete it.

## Learning
For isolated Unity Catalog securables created through a workspace anchor, Databricks automatically binds the creation workspace. Terraform should not manage that same anchor workspace as an explicit `databricks_workspace_binding`, because Terraform may destroy explicit binding resources before deleting the securable. That can remove the provider workspace's access and make external location, storage credential, catalog, or grant teardown fail. The module pattern chosen was to resolve one `workspace_binding_anchor_id`, use it in `provider_config`, leave that automatic anchor binding unmanaged, manage explicit bindings only for the remaining selected workspaces, and force replacement when the anchor changes to avoid stale automatic bindings.

## Future Use
When changing Databricks selected-workspace modules, preserve the anchor-binding contract and make grants use the same resolved anchor as the securable.
