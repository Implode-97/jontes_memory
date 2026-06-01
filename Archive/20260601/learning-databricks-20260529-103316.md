---
type: short-term-learning
topic: databricks
created: 2026-05-29 10:33:16
source: codex-conversation
confidence: medium
---

# Learning: Spark volume-read tuning preference

## Context
The user asked about tuning a heavily I/O-bound Databricks Spark workload that reads from Unity Catalog Volumes. They specifically preferred Spark configuration adjustments before code-level rewrites.

## Learning
For Databricks Spark performance discussions, start with config-level and file-layout levers when practical: file scan partition sizing, file open cost, input partition count, parallel file discovery, shuffle partitioning only when relevant, and Databricks/runtime defaults. Be clear that `spark.task.cpus` cannot be fractional and Spark cannot oversubscribe task slots per CPU through that setting.

## Future Use
When similar Databricks Spark I/O-bound questions appear, distinguish Spark file-source reads from custom code opening `/Volumes/...` paths inside tasks. Recommend Spark configs only where they affect the actual read path, and call out when conversion to Delta/table layout or per-task threaded I/O is the more realistic fix.
