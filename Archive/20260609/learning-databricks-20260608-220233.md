---
type: short-term-learning
topic: databricks
created: 2026-06-08 22:02:33
source: codex-conversation
confidence: high
---

# Learning: Keep Databricks Security Group Ports Current

## Context
While scoring the `aws-security-group` Terraform module, the current Databricks AWS networking docs were compared against the module's hard-coded egress model and examples.

## Learning
For Databricks AWS customer-managed VPC security groups, treat self-referencing all TCP/UDP rules as required safety behavior. Keep the outbound port contract precise: `8445` is current compute-to-control-plane API traffic, `8446-8451` are future/reserved, and `5432` may be needed for Lakebase depending on scope. Avoid public Terraform knobs named as broad "future extendability" unless the platform intentionally supports those ports now.

## Future Use
When reviewing or building Databricks SG modules, verify current Databricks docs before freezing the API. Prefer explicit current ports over speculative toggles, document IPv4-only CIDR behavior when using `cidr_ipv4`, and use stable caller-chosen keys for custom rule maps if those rules may change over time.
