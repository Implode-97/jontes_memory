---
type: short-term-learning
topic: databricks
created: 2026-06-01 15:42:59
source: codex-conversation
confidence: high
---

# Learning: Do not half-enable Databricks file events

## Context
During modules MR `CAV-133555`, CI failed when the sandbox catalog wrapper real apply tried to create a Databricks external location with `enable_file_events = true` but without a file event queue block.

## Learning
Databricks file events are not just a harmless boolean. In Terraform, enabling them requires modeling the queue mode, such as Databricks-managed SQS, together with the matching SNS/SQS/S3 notification IAM permissions. Until that full feature is deliberately implemented, sandbox and external-location modules should not set `enable_file_events` or grant broad `csms-*` SNS/SQS permissions by default. The default posture is to leave file events out and treat opt-in support as a separate feature.

## Future Use
When adding file events later, implement the Terraform `file_event_queue` configuration, IAM permissions, tests, and cost/governance notes together. Do not add only the boolean or only the IAM policy.
