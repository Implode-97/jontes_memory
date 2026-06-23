---
type: short-term-learning
topic: databricks
created: 2026-06-22 12:13:50
source: codex-conversation
confidence: high
---

# Learning: External-location V1 terse registry

## Context
The user refined the standalone `dbx_external_locations` plan to reduce YAML verbosity and defer storage-credential sharing complexity.

## Learning
For V1, `external_locations.yml` should be terse: `state = enabled`, `read_only = true`, and `skip_validation = true` are defaults. The registry should put `aws_iam_role_arn` directly on each external-location row and create one UC storage credential per external location. Do not add a separate storage credential map until real sharing needs appear. A data product assignment should use only `product_key` and expand to all active env-cycle service principals for that product.

## Future Use
When implementing `dbx_external_locations`, keep Terragrunt normalization responsible for expanding product keys and deriving one storage credential per location. Avoid making users repeat env cycles or boilerplate defaults in YAML.
