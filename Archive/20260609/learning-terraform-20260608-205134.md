---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:51:34
source: codex-conversation
confidence: high
---

# Learning: Prefer provider-derived facts over duplicate module inputs

## Context
During the IaC scoring run, the Databricks workspace lower module exposed both caller-provided facts and provider/data-source-resolved facts for account and metastore region consistency.

## Learning
Terraform modules should avoid public inputs for facts already owned by providers or data sources when the input only exists to re-check consistency. For Databricks workspace modules, `metastore_id` plus a provider data lookup can derive the metastore region, and `databricks_current_config` can often derive the account ID from the configured `databricks.mws` provider. Duplicated inputs such as `metastore_region` or caller-provided account IDs increase API surface and allow contradictory values.

## Future Use
When reviewing or writing Terraform/Terragrunt platform modules, prefer deriving provider-owned facts inside the module or wrapper. Keep explicit inputs for real external dependencies, but remove duplicate metadata unless it is needed for dependency drift detection and that purpose is documented.
