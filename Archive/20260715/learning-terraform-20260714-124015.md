---
type: short-term-learning
topic: terraform
created: 2026-07-14 12:40:15
source: codex-conversation
confidence: high
---

# Learning: Sandbox file events need persistent shared-role permissions and coordinated release

## Context
CAV-135209 added an opt-in Databricks managed-SQS flag across the sandbox catalog Terraform wrapper and Terragrunt catalog. This corrects the earlier same-day memory that described wrapper support as absent; the coordinated module branch now provides it.

## Learning
Each sandbox row uses `enable_file_events`, defaults to false, and maps only to `managed_sqs`. The shared regional storage-credential role must retain Databricks file-event IAM permissions even when all flags are false, because removing them before the external-location update can prevent Databricks from deleting its S3 notification, SNS topic, and SQS queue. IAM permissions alone create no resources or usage cost. AWS global list actions require `Resource = "*"`; resource-capable actions remain scoped to the sandbox bucket family and regional/account `csms-*` resources.

## Future Use
Publish the wrapper contract before pinning its new version in Terragrunt. Test enabled/default-disabled mapping, exact policy shape, retained teardown permissions, rendered inputs, and the E2E output.
