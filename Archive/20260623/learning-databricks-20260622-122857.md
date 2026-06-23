---
type: short-term-learning
topic: databricks
created: 2026-06-22 12:28:57
source: codex-conversation
confidence: high
---

# Learning: external location v1 contract

## Context
During planning for the `sandbox_catalog` Databricks Unity Catalog external-location feature, the user clarified the final v1 contract after a multi-agent review loop.

## Learning
For source-owned S3 external locations, `read_only = true` and `skip_validation = true` should be hard-coded by the wrapper, not exposed as YAML knobs. Creator principals should get a hard-coded current external-location privilege set: `READ_FILES`, `CREATE_EXTERNAL_TABLE`, and `CREATE_EXTERNAL_VOLUME`; do not implement dynamic wildcard privilege expansion. File events must be opt-in and explicitly disabled by default because Databricks enables them by default for new external locations, while source teams own S3/IAM/SQS setup. Handoff should include both Databricks `external_id` and `unity_catalog_iam_arn`.

## Future Use
When implementing this feature, keep YAML terse, avoid source-side AWS mutation, and treat runtime S3/IAM/eventing failures as source-team operational issues. Use the Databricks assume-role-policy data source only to expose trust-policy handoff metadata.
