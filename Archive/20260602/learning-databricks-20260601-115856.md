---
type: short-term-learning
topic: databricks
created: 2026-06-01 11:58:56
source: codex-conversation
confidence: high
---

# Learning: Diagnose Databricks UC S3 access before cleanup

## Context
During sandbox catalog E2E debugging, the Databricks external location failed with `AWS IAM role does not have READ permissions` even though the Terraform-rendered role trust and S3 policy looked correct and the module-level real apply passed.

## Learning
Do not treat unique names as the root fix for UC S3 READ failures. Separate the failure into STS assume-role, S3 authorization, wrong credential/external ID, file-events validation, and fresh workspace/metastore readiness. Databricks' Terraform guide documents creating the storage credential before the IAM role because the credential external ID is needed for trust, so that order alone is not evidence of a bug. The missing proof is failure-time diagnostics before cleanup.

## Future Use
For future CI failures, collect IAM role/trust/inline policy, bucket policy, IAM simulation, CloudTrail STS/S3 evidence, and Databricks storage credential validation before destroying resources. Explain module-test success by checking whether it used a warmed long-lived workspace/metastore instead of the full fresh E2E bootstrap path.
