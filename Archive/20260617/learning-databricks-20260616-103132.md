---
type: short-term-learning
topic: databricks
created: 2026-06-16 10:31:32
source: codex-conversation
confidence: high
---

# Learning: cuDF Spark Naming For Spark RAPIDS

## Context
While clarifying RAPIDS setup for Databricks, the user noticed that current searches and links now land in NVIDIA repositories named `cudf-spark` rather than the older `spark-rapids` name.

## Learning
NVIDIA appears to have moved the Spark RAPIDS source repository and GitHub Pages project naming toward `cudf-spark`: `github.com/NVIDIA/spark-rapids` resolves to `github.com/NVIDIA/cudf-spark`, and project docs are also available at `nvidia.github.io/cudf-spark`. This is not a different Databricks feature. The user-facing name in NVIDIA Docs remains `RAPIDS Accelerator for Apache Spark`, the Maven artifacts are still `rapids-4-spark_<scala>`, the Spark plugin class remains `com.nvidia.spark.SQLPlugin`, and configuration keys remain `spark.rapids.*`.

## Future Use
For future Spark GPU work, search both `RAPIDS Accelerator for Apache Spark` and `cudf-spark`. Treat `cudf-spark` as the repo/project naming for the same cuDF-backed Spark RAPIDS plugin unless NVIDIA explicitly documents a product split.
