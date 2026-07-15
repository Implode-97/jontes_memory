---
type: short-term-learning
topic: terraform
created: 2026-07-14 00:00:00
source: codex-conversation
confidence: high
---

# Learning: Databricks managed file-events list permissions need wildcard resources

## Context
During a review of `data-platform-terraform-modules`, the sandbox catalog wrapper added Databricks-managed SQS file-event IAM permissions for external locations.

## Learning
For the AWS policy statements that support Databricks managed file-event setup, `sqs:ListQueues` and `sns:ListTopics` do not support resource-level permissions and must be granted with `Resource = "*"`. Scoping those list actions to `arn:aws:sqs:...:csms-*` or `arn:aws:sns:...:csms-*` looks least-privilege but does not authorize the actions, so Databricks automatic queue/topic discovery can still fail with access denied when `enable_file_events = true`.

## Future Use
When reviewing or implementing Databricks managed SQS file-events IAM, split global list actions into a wildcard-resource statement and keep queue/topic/bucket-scoped actions in separate statements with ARN resources.
