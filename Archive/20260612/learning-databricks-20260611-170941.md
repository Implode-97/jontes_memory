---
type: short-term-learning
topic: databricks
created: 2026-06-11 17:09:41
source: codex-conversation
confidence: high
---

# Learning: Databricks CLI Account Groups V2 Uses Account API

## Context
The user asked whether an access-platform service principal with `Group: Manager` on all groups could use `databricks account groups-v2 list` without account admin privileges.

## Learning
The Databricks CLI `databricks account groups-v2 ...` command is generated against `AccountClient.GroupsV2`; in SDK v0.141.0 it calls account-host paths like `/api/2.0/accounts/{account_id}/scim/v2/Groups`. Databricks docs say workspace admins and group managers should use the workspace-domain account SCIM proxy at `/api/2.0/account/scim/v2/`, while account admins use the account-domain `/api/2.1/accounts/{account_id}/scim/v2/` path. Therefore, do not assume `Group: Manager` is enough for the `databricks account groups-v2` command.

## Future Use
For least-privilege group membership automation by a non-account-admin service principal, recommend a workspace-host profile plus `databricks api get/patch /api/2.0/account/scim/v2/Groups...` or direct HTTP calls. Avoid account CLI commands and account-host Terraform resources unless the automation identity is intentionally an account admin.
