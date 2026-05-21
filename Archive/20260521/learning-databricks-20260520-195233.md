---
type: short-term-learning
topic: databricks
created: 2026-05-20 19:52:33
source: codex-conversation
confidence: high
---

# Learning: Workspace admin bootstrap group belongs in the workspace wrapper

## Context
This came from the Databricks Terragrunt E2E bootstrap discussion after the deployer service principal needed both metastore-level and workspace-level authority. The user decided to keep the suffix `admin` for permission-bearing platform groups and avoid overloading team role names such as `owner`.

## Learning
For Databricks workspace creation, the workspace wrapper should create a platform-prefixed workspace admin group such as `platform-<workspace_name>-workspace-admins`, assign that group `ADMIN` on the workspace, and add the currently authenticated deployment service principal to it by deriving the SP from the Databricks provider context. This keeps elevated workspace access tied to a platform/admin group rather than team user groups.

## Future Use
When adding workspace-scoped features such as clusters, policies, jobs, or workspace provider resources, treat this workspace admin group as baseline workspace readiness. Do not add deployer SP IDs as Terragrunt inputs if the provider can derive the current identity.
