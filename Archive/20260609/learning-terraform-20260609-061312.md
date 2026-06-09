---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:13:12
source: codex-conversation
confidence: high
---

# Learning: Catalog outputs should avoid duplicate API shapes

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-catalog/outputs.tf`, reviewers compared observed wrapper consumers against Terraform output API guidance and Databricks workspace-binding behavior.

## Learning
For the Databricks workspace catalog module, preserve outputs that have real caller or lifecycle value: catalog `name` for grant modules, wrapper-facing owner/storage/properties/force-destroy reporting where used, and `workspace_bindings` because explicit binding metadata excludes Databricks' automatic creation-workspace binding. The broad duplicate `catalog` object and unused provider-fact scalars such as `id`, `metastore_id`, `storage_location`, `access_mode`, and `isolation_mode` should score lower before API freeze unless real consumers appear.

## Future Use
When reviewing adjacent catalog, external-location, or storage-credential outputs, separate stable downstream contracts from test-shaped or diagnostic mirrors. Prefer one output shape per fact, keep explicit binding visibility when it explains unmanaged automatic bindings, and avoid freezing provider internals just because mock tests assert them.
