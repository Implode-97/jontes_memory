---
type: short-term-learning
topic: databricks
created: 2026-06-01 10:28:10
source: codex-conversation
confidence: high
---

# Learning: Databricks File Events Are Default-On For External Locations

## Context
While hardening sandbox catalog external-location creation after READ validation failures, the user clarified that file events should be enabled by default rather than explicitly disabled.

## Learning
For Databricks AWS Unity Catalog external locations, file events are the expected default-on behavior. Terraform modules should model that default with `enable_file_events = true` unless a caller deliberately opts out. When file events are enabled, the storage credential IAM role should include Databricks-managed file event permissions for S3 bucket notifications and `csms-*` SNS/SQS resources, not only the ordinary S3 read/write permissions.

## Future Use
When creating or reviewing Databricks external-location modules, check both the Terraform `enable_file_events` setting and the IAM policy contract. Avoid recommending file-event opt-out as a default workaround unless the user explicitly wants a temporary approval-process exception.
