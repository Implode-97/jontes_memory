---
scope: domain
domain: databricks
topics: [identities, groups, workspace-assignment, entitlements, admin, bootstrap]
stability: mixed
last-reviewed: 2026-07-10
---

# Databricks Governance

## Team Identities

- Move from one broad team group toward account-level identities with nested role groups.
- Canonical roles include `_owner`, `privileged_contributor`, `contributor`, `new_contributor`, `reader`, and `consumer`.
- Prefer `snake_case` for team keys, role names, YAML fields, and generated IaC identifiers.
- Identity ownership and workspace membership are separate feature boundaries.
- Group membership modules are lookup-only: resolve existing users, groups, and service principals, then create typed membership resources.
- Do not let membership helpers create identities, entitlements, Unity Catalog permissions, or polymorphic abstractions.
- Put platform naming policy in wrappers that derive display names; leaf modules own only null, whitespace, and length hygiene.
- Team identity guards must preserve lifecycle protection, final-row deletion protection, prior-state output lookup, and all-violations reporting.

## Workspace Assignment

- Keep account and workspace provider responsibilities explicit: account groups and MWS permission assignments use account context; workspace lookup and entitlements use a workspace alias.
- Preserve `workspace_team_assignments` as the stable downstream contract. Treat entitlement diagnostics and provider-owned resource IDs as internal unless consumed.
- Default team roles map to workspace access as `consumer -> workspace_consume`, `reader -> sql_access`, and `contributor -> workspace_access`.
- Active placement rows (`enabled` or `destroy_pending`) must reference enabled teams; ignore destroyed placements.
- Every guard that consumes generated inputs must validate required `teams`, `workspace_teams`, and lifecycle output shapes itself or run after a required-input guard.
- Never treat a missing or malformed lifecycle output as proof of first apply.

## Platform Bootstrap And Admin Groups

- Account administration and Unity Catalog privileges are distinct; an account deployer does not automatically own metastore objects.
- Add the deployment service principal to the existing metastore admin group instead of making it metastore owner.
- Account-scoped wrappers accept an explicit deployer application ID and resolve it with account-compatible service-principal data sources.
- Source the deployer ID from the CI identity, normally `DATABRICKS_CLIENT_ID`; use account-scoped configuration if multi-account deployment becomes real.
- Workspace wrappers create a platform-prefixed workspace admin group, assign it `ADMIN`, and include the deployment principal.
- Use `admin` for permission-bearing platform groups; do not overload team roles such as `owner`.

## Permission Ordering

- Permission assignment can race metastore assignment and identity federation in new workspaces.
- Depend on full workspace or metastore-assignment completion, not only a known workspace ID.
- If explicit ordering is insufficient, investigate eventual consistency and add bounded readiness polling before broadening contracts.

## Current State — Verify

- June 2026 evidence showed workspace admins and group managers could use workspace-domain account SCIM proxy paths.
- Terraform account-group resources and `databricks account groups-v2` still required account-admin context; re-check provider, CLI, and target-account behavior.
- Databricks system-group entitlement hardening is moving toward locked behavior. Treat Terraform management of the `users` system group as legacy/migration work and re-check current documentation.
