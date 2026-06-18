---
type: short-term-learning
topic: terragrunt
created: 2026-06-17 13:59:36
source: codex-conversation
confidence: high
---

# Learning: Data Product Catalog V1 Contract

## Context
The user planned a new Data Platform IaC story for data product catalogs in `/Users/jnyjc2/code/vibe_code/sandbox_catalog`, then created Jira Story `CAV-134270`.

## Learning
V1 should create a standalone `dbx_data_product_catalogs` feature, not extend the sandbox catalog wrapper. The regional input file is `data_products.yml`, with one product row containing an explicit `env_cycles` map. Product-level `compute_profile_ids` and provider-neutral raw `oidc_federation` entries are copied to each env-cycle service principal. Each `product_key + env_cycle` gets one service principal, federation policies, bucket, external location, and catalog. The metastore owner group owns the catalog; the service principal gets `ALL_PRIVILEGES` and `MANAGE`. `break_glass_enabled` lives per env cycle, defaults true except for `prd`, and grants `grp_team_<owner_team_key>_owner`. Lifecycle states are `enabled`, `destroy_pending`, and `destroyed`; row removal is guarded until a destroyed apply.

## Future Use
When implementing `CAV-134270`, treat older `data-product-catalog-plan.md` as historical context only. Reuse sandbox patterns for storage pools, lifecycle guards, and tests where the analogy is clean, but keep workspace assignment and catalog bindings out of this feature.
