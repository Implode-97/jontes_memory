---
type: short-term-learning
topic: terraform
created: 2026-05-18 17:49:00
source: codex-conversation
confidence: high
---

# Learning: derive provider-owned Terraform facts

## Context
Retroactive scan of Codex Terraform and Databricks module sessions from March 2026 showed a repeated preference around module inputs and provider data sources.

## Learning
For this user's platform Terraform work, avoid passing account, region, Databricks account, and similar provider-owned facts as ordinary inputs when they can be reliably derived. Prefer `aws_caller_identity`, `aws_region`, and Databricks provider data sources such as current config or metastore lookup when those values are part of the configured provider context or implied by a required ID. Also hard-code the commercial AWS partition as `aws`; do not add `aws_partition` inputs or `data.aws_partition` just to support China or GovCloud.

## Future Use
When reviewing or implementing Terraform modules, challenge duplicate sources of truth. Keep explicit inputs for naming tokens and real feature choices, but derive deployment facts from providers or required resource IDs.
