---
type: short-term-learning
topic: databricks
created: 2026-05-21 11:31:00
source: codex-conversation
confidence: high
---

# Learning: Workspace permission assignment must wait for identity federation

## Context
In the Databricks deployer admin-access module MR, the workspace wrapper real-provider apply failed after the deployer SP ID was corrected. The first live error was `Permission assignment APIs are not available for this workspace` on `databricks_mws_permission_assignment`.

## Learning
For a newly created Databricks workspace, account/workspace permission assignment can race ahead of the metastore assignment that enables identity federation. The workspace wrapper should make `databricks_mws_permission_assignment` depend on the full workspace module, not only on `module.workspace.workspace_id`, because that output can be known before the child module's `databricks_metastore_assignment` has completed.

## Future Use
When wiring workspace admin groups or principals into new workspaces, ensure the permission assignment waits for the metastore assignment. If the same error persists after explicit dependency ordering, investigate eventual consistency before adding broader module changes.
