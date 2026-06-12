---
type: short-term-learning
topic: databricks
created: 2026-06-11 17:02:35
source: codex-conversation
confidence: high
---

# Learning: Databricks Group Management Without Account Admin

## Context
The user asked whether Databricks groups can be managed without Databricks account admin rights and how to do it.

## Learning
As of June 11, 2026, Databricks docs distinguish UI/direct REST capability from Terraform provider behavior. Workspace admins can create account groups, add members, and assign groups to their workspace through workspace admin settings or the workspace-domain account SCIM proxy at `/api/2.0/account/scim/v2/`. Non-admin group managers can manage membership and group-manager role delegation for groups they manage through the same workspace-domain API surfaces. However, the Terraform provider's `databricks_group` account mode targets account-host SCIM paths like `/api/2.0/accounts/{account_id}/scim/v2/Groups`, so account-level group creation through the provider should still be treated as account-admin automation unless proven otherwise in the target account/provider version.

## Future Use
For least-privilege Databricks group workflows, recommend workspace admin UI/direct REST or IdP-managed groups first. For Terraform, keep account group creation/membership in an account-admin stack, or use workspace-level `databricks_permission_assignment` with `group_name` for assigning existing account groups to workspaces.
