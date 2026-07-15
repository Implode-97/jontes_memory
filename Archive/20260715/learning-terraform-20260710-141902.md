---
type: short-term-learning
topic: terraform
created: 2026-07-10 14:19:02
source: codex-conversation
confidence: high
---

# Learning: Data product lifecycle must cover the backing S3 bucket

## Context
The CAV-134270 data product catalog review traced the `enabled -> destroy_pending -> destroyed` contract through the Terraform wrapper and the shared secure S3 bucket module.

## Learning
Setting Databricks catalog and external-location `force_destroy` is insufficient for a data-bearing product. The current secure S3 primitive does not expose `aws_s3_bucket.force_destroy`, while the wrapper removes the bucket module when the row reaches `destroyed`. A non-empty bucket can therefore block the promised cleanup after the Unity Catalog resources are removed. Empty real-provider tests do not exercise this failure.

## Future Use
For lifecycle-managed catalog storage, either add a default-false, resource-shaped S3 `force_destroy` input and enable it only during the guarded `destroy_pending` stage, or implement and document a guarded bucket-emptying path. Add a test that asserts the S3 setting as well as the Databricks force flags.
