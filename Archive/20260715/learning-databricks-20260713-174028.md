---
type: short-term-learning
topic: databricks
created: 2026-07-13 17:40:28
source: codex-conversation
confidence: high
---

# Learning: Separate team-bound and standalone Databricks service principals

## Context
CAV-133992 added enterprise Databricks service principals across the Terraform modules and Terragrunt catalog repositories.

## Learning
Team-bound identities belong in `dbx_team_identities`: create contributor, privileged-contributor, and owner SPs per deployable region; add each SP to its matching group; grant that group `roles/servicePrincipal.user`; and rely on inherited group entitlements and data privileges. Standalone CI identities instead need a regional YAML lifecycle, the name `sp_<region>_team_<team>_ci_<name>`, one or more generic OIDC policies with account-ID audience, explicit workspace `USER` plus `workspace_access`, sandbox `USE_CATALOG` plus `CREATE_SCHEMA`, and explicit fixed catalog-read requests. External-location creator grants remain owned downstream by the external-location feature.

## Future Use
Preserve these three ownership boundaries. Recommend one standalone identity per permission/lifecycle boundary without enforcing repository cardinality. Retire external assignments first, then the standalone SP, then its sandbox.
