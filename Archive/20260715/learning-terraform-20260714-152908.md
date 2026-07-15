---
type: short-term-learning
topic: terraform
created: 2026-07-14 15:29:08
source: codex-conversation
confidence: high
---

# Learning: Managed file-events Unsubscribe needs wildcard IAM resource

## Context
A review of the CAV-135209 sandbox file-events IAM policy found that the scoped teardown statement followed Databricks' example but conflicted with AWS's service authorization model.

## Learning
Amazon SNS does not define a resource type for `sns:Unsubscribe`. IAM therefore authorizes that action only when the statement uses `Resource = "*"`; a `csms-*` topic ARN does not authorize it, even though the subscription belongs to that topic. Keeping `sns:Unsubscribe` in a teardown statement scoped to SNS and SQS ARNs can cause Databricks to receive `AccessDenied` while disabling file events or deleting an external location. `sns:DeleteTopic` and `sqs:DeleteQueue` remain resource-scopeable.

## Future Use
Place `sns:Unsubscribe` in a wildcard-resource statement, potentially beside `sqs:ListQueues` and `sns:ListTopics`, while retaining scoped statements for resource-capable teardown actions. Assert both actions and resources in policy tests rather than only action names.
