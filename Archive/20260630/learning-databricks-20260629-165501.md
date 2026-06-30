---
type: short-term-learning
topic: databricks
created: 2026-06-29 16:55:01
source: codex-conversation
confidence: high
---

# Learning: Show Spark Native Versus UDF Read Performance Paths

## Context
While reviewing a Databricks Unity Catalog volumes and FUSE diagram, the user pointed out that the existing communication/auth diagrams did not show whether native Spark reads would perform differently from opening each file inside a Spark UDF.

## Learning
For Databricks UC volume diagrams involving Spark reads, explicitly separate the performance path from the authentication path. Native `spark.read` over `/Volumes` or `dbfs:/Volumes` should be shown as a Spark datasource scan where the driver plans listing, filters, file partitions, and executor reads. UDF-based file opening should be shown as custom task I/O where Spark sees rows or paths, not the actual file scan, so splitting, grouping, pruning, metrics, and failure behavior differ.

## Future Use
When explaining Spark access to UC volumes, include a performance-shape diagram or section if the user is comparing native Spark readers with per-file UDF logic. Avoid implying that both paths are equivalent just because both run on executors or touch the same backing cloud storage.
