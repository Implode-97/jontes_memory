---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:18:48
source: codex-conversation
confidence: high
---

# Learning: Data product catalog module boundary

## Context
During a read-only investigation of `sandbox_catalog`, the user asked how to build SP-owned Databricks data product catalogs from or near existing Terraform modules.

## Learning
The current repo has reusable low-level primitives for Databricks service principals, storage credentials, external locations, catalogs, single-principal catalog grants, S3 buckets, IAM roles, team identities, workspace assignments, and compute policies. It does not yet have a `dbx_data_product_catalogs` unit, a data product wrapper, a service-principal federation-policy module, or a product service-principal workspace-assignment contract. Product catalogs should not be folded into the sandbox wrapper because sandbox team grants, lifecycle tombstones, and platform catalog ownership differ from product SP ownership.

## Future Use
For data product catalog work, recommend a new sibling wrapper/unit that reuses neutral storage/catalog primitives, adds `databricks_service_principal_federation_policy`, exposes product identity placement outputs, and lets Terragrunt wire workspace assignment and compute policy access.
