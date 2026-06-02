---
type: short-term-learning
topic: databricks
created: 2026-06-01 11:49:16
source: codex-conversation
confidence: high
---

# Learning: Diagnose Databricks External Location READ Failures By Failure Mode

## Context
During sandbox catalog E2E debugging, the user pushed back that unique deployment names are not a fundamental fix for Databricks external-location READ failures. The useful distinction is where the validation chain actually failed.

## Learning
For Databricks AWS Unity Catalog external-location READ failures, separate at least five modes before changing code: Databricks cannot assume the IAM role, Databricks can assume but S3 denies read/list, the storage credential has the wrong role ARN or external ID, the workspace/metastore assignment is not ready, or Terraform generated a policy with wrong input variables. A passing module real apply against a warmed-up shared workspace/metastore does not falsify failures in a Terragrunt E2E topology that creates fresh workspace/metastore infrastructure.

## Future Use
Use Databricks validate-storage-credentials output, storage credential state/API fields, IAM trust/inline policy, bucket policy, IAM simulation, and CloudTrail STS/S3 evidence to classify the failure before recommending sleeps, naming changes, or policy edits.
