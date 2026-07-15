---
type: short-term-learning
topic: databricks
created: 2026-07-10 15:51:12
source: codex-conversation
confidence: high
---

# Learning: Keep provider-owned OIDC policy metadata out of data product YAML

## Context
The CAV-134270 data product OIDC contract was simplified after checking the installed Databricks provider schema and the documented service-principal federation defaults.

## Learning
Data product environment `oidc_federation` entries should contain only `issuer`, `subject`, optional `subject_claim`, and optional `description`. Do not require a caller-defined policy name or audience. The Databricks provider can generate `policy_id`, and Databricks defaults an omitted audience to the Databricks account ID. In the wrapper, track multiple policies by their list index for Terraform resource identity and expose applied policies keyed by the generated policy ID.

## Future Use
Keep the live YAML concise and avoid repeating the same Databricks account ID in every environment. Treat the generated policy ID and default audience as provider-owned facts. Preserve tests that verify these inputs are absent from rendered Terragrunt configuration and that live E2E returns a generated ID and non-empty audience.
