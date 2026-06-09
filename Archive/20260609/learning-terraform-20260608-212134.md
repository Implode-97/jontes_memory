---
type: short-term-learning
topic: terraform
created: 2026-06-08 21:21:34
source: codex-conversation
confidence: high
---

# Learning: Keep IAM role primitive inputs generic and safety-shaped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-iam-role/variables.tf`, the user asked for an AWS IAM API safety and naming perspective focused on readability, KISS, YAGNI, maintainability, and missing safety-critical checks.

## Learning
For the generic lower-level `aws-iam-role` module, keep policy inputs named for their IAM role in the resource rather than current Databricks cross-account consumers. `crossaccount_policy_json` is misleading when the module attaches a generic inline permissions policy; prefer a name such as `inline_policy_json` or `permissions_policy_json`. Syntax-only JSON validation is acceptable when callers pass provider/data-source generated IAM documents, but it should not be presented as semantic IAM policy validation.

## Future Use
In future reviews of this module, prioritize narrow API contract cleanup over policy-builder abstractions. Validate optional `permissions_boundary_arn` only for the commercial AWS IAM policy ARN shape when non-null, and avoid fake partition flexibility unless the platform explicitly targets other AWS partitions.
