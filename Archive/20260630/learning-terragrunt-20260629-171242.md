---
type: short-term-learning
topic: terragrunt
created: 2026-06-29 17:12:42
source: codex-conversation
confidence: high
---

# Learning: external locations V1 is team-only

## Context
While rebasing and simplifying the `dbx_external_locations` feature, the user chose the long-term boundary where external locations do not depend on data product catalogs for V1.

## Learning
`dbx_external_locations` should remain a standalone regional `dbcore` unit for source-owned external locations. V1 grants creator privileges only to team contributor groups derived from `assignments.teams`. It must not depend on `dbx_data_product_catalogs`, read `dependency.data_product_catalogs.outputs.data_products`, or expand product service-principal grants. `assignments.data_products` is unsupported in this feature for now and should fail clearly at the guard boundary.

## Future Use
Do not reintroduce data-product dependencies into the external-location unit as a `try()` or apply mock. If product service principals need external-location creator grants later, implement that as a separate bridge unit depending on both external locations and data product catalogs.
