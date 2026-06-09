---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:01:26
source: codex-conversation
confidence: high
---

# Learning: AWS security group module review shape

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-security-group/main.tf`, the user asked for a Terraform maintainability lens focused on resource shape, local transformations, state/address stability, naming, KISS, and YAGNI.

## Learning
The module's separate `aws_vpc_security_group_*_rule` resources and named base/optional egress maps are a defensible, readable Terraform shape. The cleanup pressure is on the public contract: Databricks port wording changed around `8445`, `8446-8451`, and conditional Lakebase `5432`; `enable_future_extendability_egress` can look like future-proofing unless aligned to current Databricks requirements; and `additional_egress_tcp_rules` as a list creates index-based custom rule addresses.

## Future Use
For future reviews, preserve the separate-rule state model and per-CIDR expansion when multiple CIDRs are required. Prefer stable named custom egress rules if the escape hatch remains, and verify Databricks security group port docs before judging required versus optional egress ports.
