---
type: short-term-learning
topic: terragrunt
created: 2026-06-26 11:22:53
source: codex-conversation
confidence: high
---

# Learning: Keep Dependency Outputs Out Of Terragrunt Locals

## Context
While implementing `dbx_external_locations` in `data-platform-terragrunt-catalog`, generated-unit HCL validation failed when `dependency.data_product_catalogs.outputs.*` was referenced from `locals`.

## Learning
In explicit Terragrunt stack-generated units, dependency outputs may be valid in `inputs` and `generate` blocks but not in `locals`. For `dbx_external_locations`, team-only normalization stayed in locals, while data-product service-principal expansion from `dbx_data_product_catalogs.outputs.data_products` was moved into the wrapper `inputs.creator_principals` expression and the generated guard JSON. This kept HCL validation stable and still allowed the unit to expand a `product_key` assignment into enabled env-cycle service principal application IDs.

## Future Use
When building new generated-stack units, keep dependency-free registry normalization in `locals`. Put direct `dependency.*.outputs` reads in `inputs` or generated files, and validate the generated unit with `terragrunt hcl validate` after stack generation.
