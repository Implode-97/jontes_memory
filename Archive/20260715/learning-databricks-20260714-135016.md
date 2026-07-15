---
type: short-term-learning
topic: databricks
created: 2026-07-14 13:50:16
source: codex-conversation
confidence: high
---

# Learning: Estimate Databricks file-event messaging from file changes

## Context
While assessing whether Databricks-managed file events should be enabled for large S3-backed Unity Catalog locations, an AWS usage report showed aggregate S3 request units and account-wide SQS and S3 `List` operations.

## Learning
Do not treat aggregate S3 request counts as `ObjectCreated` event counts. Reads, writes, multipart operations, lists, control-plane calls, deletes, and other request classes have different relationships to S3 event notifications. SQS `ListQueues` activity and S3 operations such as `ListJobs`, `ListLensConfigurations`, and `ListAllMyBuckets` are inventory/control-plane traffic and do not measure file changes. Databricks file-event costs are driven by creates, updates, and deletes, plus SQS receive/delete polling behavior, retries, and batching—not stored bytes or object size.

## Future Use
Estimate per bucket or external location using S3 Storage Lens activity metrics or existing CloudTrail data events. Validate with a representative pilot and inspect SQS sent, received, deleted, and empty-receive metrics before setting enterprise-wide defaults.
