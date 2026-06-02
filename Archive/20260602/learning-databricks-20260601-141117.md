---
type: short-term-learning
topic: databricks
created: 2026-06-01 14:11:17
source: codex-conversation
confidence: high
---

# Learning: UC catalog deletion is catalog-scoped

## Context
The user asked whether deleting a Databricks Unity Catalog catalog can be authorized by a metastore-wide permission, or whether ownership/MANAGE on the catalog is needed.

## Learning
For ordinary principals, catalog deletion is catalog-scoped: the principal must be the catalog owner, or have both `MANAGE` and `USE CATALOG` on that catalog. `CREATE CATALOG` and other metastore-level grants are not enough, because metastore-level privileges control metastore-scoped operations and do not inherit into catalogs. `MANAGE` is applicable to catalogs and schemas as containers, but not as a generic metastore-wide privilege; `ALL PRIVILEGES` also does not include `MANAGE`.

## Future Use
For Terraform/Databricks cleanup failures, check whether the deployer still owns the catalog or has explicit `MANAGE` plus `USE CATALOG`. Do not assume account admin, workspace admin, or `CREATE CATALOG` on the metastore can destroy arbitrary catalogs unless the principal is also a metastore admin or has catalog-scoped authority.
