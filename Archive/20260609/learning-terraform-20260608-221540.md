---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:15:40
source: codex-conversation
confidence: high
---

# Learning: AWS Security Group Variable API Stability

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-security-group/variables.tf`, the user asked for a Terraform validation and state-stability persona focused on API stability, validation correctness, and avoidable state churn.

## Learning
For this module, the highest-value cleanup is in the public variable contract, not the separate security-group rule resource shape. `additional_egress_tcp_rules` as a list is unstable because `main.tf` keys custom rules as `custom_${index}`; prefer caller-chosen stable map keys before the interface is treated as durable. `internet_egress_cidr_blocks` feeds `cidr_ipv4`, so validation should be IPv4-specific, duplicate-aware, and preferably canonical enough to avoid confusing address keys or provider canonicalization drift. Port variables should reject fractional numbers, and security group names should reject padded or whitespace-only values rather than silently trimming.

## Future Use
In future reviews, preserve simple explicit validations and tags-as-plain-map unless requirements demand more. Push for stable map keys, tight IPv4 CIDR validation, integer port checks, and clearer Databricks port naming before this module is depended on by persistent state.
