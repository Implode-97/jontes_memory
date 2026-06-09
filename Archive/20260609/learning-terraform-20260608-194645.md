---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:46:45
source: codex-conversation
confidence: high
---

# Learning: exact policy document tests

## Context
During the multi-agent scoring sweep, the sandbox catalog wrapper's `policy_document_real_plan.tftest.hcl` was reviewed against Terraform test behavior and AWS IAM/S3 policy documentation.

## Learning
Use a real AWS provider plan test for `aws_iam_policy_document` when mocked-provider tests cannot prove rendered IAM JSON. It is a good pattern to pair real AWS policy rendering with mocked Databricks providers and fake static credentials plus validation skips, as long as no real infrastructure or account calls are needed. For security-sensitive policy shape, existence checks are weaker than they look: `contains`/`anytrue` can pass even if extra broad resources, actions, or statements are added.

## Future Use
When testing IAM policy documents, prefer exact normalized assertions for each expected statement: Sid uniqueness, `Effect`, complete action set, and complete resource set. Use subset checks only for low-risk smoke tests, and make error messages say "includes" unless uniqueness or exactness is actually asserted.
