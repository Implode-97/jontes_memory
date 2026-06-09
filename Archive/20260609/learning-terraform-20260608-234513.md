---
type: short-term-learning
topic: terraform
created: 2026-06-08 23:45:13
source: codex-conversation
confidence: high
---

# Learning: Serverless budget policy invalid tests should stay compact

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-serverless-budget-policy/tests/invalid_inputs_mock_plan.tftest.hcl`, the user asked for a pragmatic platform maintainer view focused on whether the test protects the input contract without becoming noisy.

## Learning
For this module, invalid mock plan coverage is useful when it directly mirrors the current `var.policies` validation: non-empty `policy_name`, positive `workspace_ids`, and any remaining non-empty `policy_type` contract. Repeating a full prepared policy object for each one-field invalid case is understandable but lower-KISS because `user_principals` and `tags` are not validated by this file and mostly add fixture noise. `policy_type` validation is also lower-value if it remains caller metadata rather than a Databricks resource-owned field.

## Future Use
In future reviews, accept the test as shippable but prefer narrow cleanup: use a minimal valid policy fixture or a local/helper pattern if Terraform test syntax supports it cleanly, keep only validation cases that match durable module contract fields, and avoid adding invalid cases for unvalidated tags or principals unless the contract changes.
