---
scope: domain
domain: databricks
topics: [unity-catalog, catalogs, external-locations, volumes, workspace-bindings, storage]
stability: mixed
last-reviewed: 2026-07-10
---

# Databricks Unity Catalog

## Sandbox Catalogs

- Give each team one dev-workspace sandbox by default. Add regional sandboxes only for justified transfer-cost or GPU locality needs.
- A sandbox catalog combines S3 root storage, a storage credential, an external location, and catalog root configuration.
- Set catalog ownership to the platform or regional metastore owner group; grant the team owner/admin principal `ALL_PRIVILEGES` without `MANAGE`.
- `ALL_PRIVILEGES` does not grant catalog deletion or grant management. Delete requires ownership, or `MANAGE` plus `USE CATALOG`, unless the caller is a metastore admin.
- Sandbox catalogs are collaboration spaces, not productionized products. Do not grant other teams access by default.
- Keep declarations regional and let the team role create schemas and tables; child-object owners decide narrower sharing.
- Correct IAM and credential configuration can still race AWS or fresh workspace/metastore readiness. Prefer bounded readiness checks over permanent sleeps.

## Workspace Bindings

- `workspace_ids` is the caller access contract: non-empty means exactly those workspaces; empty means deliberate all-workspaces/open mode.
- Workspace-level providers automatically anchor isolated securables to the creating workspace.
- Lower modules own anchor subtraction and validation through `provider_workspace_id` or `workspace_binding_anchor_id`; callers pass the intended workspace set unchanged.
- Leave the automatic anchor binding unmanaged. Manage explicit bindings only for other selected workspaces.
- Explicit bindings provide safe destroy ordering: remove non-anchor access before deleting the securable.
- Force replacement when the anchor changes so stale automatic bindings do not survive.
- Configure workspace providers with `host`; do not confuse provider configuration with module inputs such as `workspace_ids` or `provider_workspace_id`.

## Storage Credentials And External Locations

- Document the external-ID bootstrap: placeholder IAM trust, Databricks storage credential, returned external ID, then finalized trust and self-assumption policy.
- Wrappers may create credentials with `skip_validation = true`; later external-location validation can remain meaningful.
- Keep external-location and storage-credential outputs narrow: names, external IDs, bindings, validation flags, and lifecycle markers.
- For source-owned V1 external locations, source teams own S3 URLs, IAM roles, bucket policy, and optional queues; the platform owns Databricks credentials, locations, and grants.
- Hard-code V1 `read_only = true` and `skip_validation = true` rather than exposing unstable YAML switches.
- Default to open/all-workspaces access and grant only `CREATE_EXTERNAL_TABLE` and `CREATE_EXTERNAL_VOLUME` to derived team contributor groups.
- Do not grant storage-credential privileges or default `READ_FILES`, `WRITE_FILES`, `MANAGE`, `ALL_PRIVILEGES`, `CREATE_MANAGED_STORAGE`, or `EXTERNAL_USE_LOCATION`.
- Keep `external_locations.yml` terse. Derive one credential per role and reject unsupported `assignments.data_products` at the guard boundary.
- File events are a complete opt-in object: `null` when disabled, `managed_sqs` without a queue URL, or `provided_sqs` with a caller-owned queue URL.
- Canonicalize an S3 URL once from caller input and reuse it for desired external-location and catalog storage-root configuration. Never feed provider-normalized output formatting back into desired inputs.

## Volumes And Data Access

- Unity Catalog is the governance source of truth, not a literal prohibition on every physical S3 path.
- Direct access to managed storage remains prohibited. Use governed tables, volumes, credential vending, or controlled regional training stacks when raw cloud access is necessary.
- For data inside an external volume, cloud URI access is governed by volume privileges, not parent external-location `READ_FILES` or `WRITE_FILES`.
- Normal volume reads require `USE CATALOG`, `USE SCHEMA`, and `READ VOLUME`; writes also require `WRITE VOLUME`.
- Native Spark reads through `/Volumes` or `dbfs:/Volumes` use Spark file planning and executor reads. File I/O inside a UDF is custom task I/O with different partitioning and failure behavior.
- Productize S3 exceptions with audit, TTL, owner, tags, and registry-driven controls. Do not use broad `sts:AssumeRole` as a governance escape hatch.
- Enforce restrictions on the final assumed-role session, target policies or boundaries, bucket policies, organization guardrails, or Unity Catalog primitives.

## Data Product Catalogs

- Keep productionized data products separate from team sandboxes and assign each a repository-bound service principal with OIDC/federation.
- Model service principals, federation, catalog ownership, and target environments explicitly; team groups do not directly control production products.
- Each `product_key + env_cycle` owns one service principal, federation policies, bucket, external location, and catalog.
- Grant product principals by application ID, not display name. Break-glass uses `grp_team_<owner_team_key>_owner` after team identities exist.
- Keep federation policy names lowercase and hyphenated and cap policies at 20 per product environment.
- Expose one canonical `data_products` output per environment with catalog, service principal, grants, and federation policy data.
- Keep lifecycle and shared-region outputs separate; do not publish empty binding outputs before workspace assignment exists.
- Do not carry compute-profile intent in the catalog feature until a current resource consumes it.

## Catalog Renames

- Stable registry keys can preserve Terraform addresses across a rename but do not solve provider name-based state behavior.
- Until native rename is proven, rename through API/UI and explicitly remove/import catalog and dependent name-keyed state.

## Current State — Verify

- Databricks provider URL normalization may add a trailing slash and force replacement when downstream desired paths reuse refreshed outputs; retain the trailing-slash regression test.
- Catalog rename support exists in platform APIs, but Terraform provider behavior must be re-checked before replacing explicit migration guidance.
