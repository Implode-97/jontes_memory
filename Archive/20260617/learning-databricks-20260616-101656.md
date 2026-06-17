---
type: short-term-learning
topic: databricks
created: 2026-06-16 10:16:56
source: codex-conversation
confidence: high
---

# Learning: Spark RAPIDS Databricks Support As Of June 2026

## Context
During RAPIDS investigation for `/Users/jnyjc2/code/innovation-week/RAPIDS`, current NVIDIA and Databricks docs were checked for the easiest way to run NVIDIA RAPIDS on Databricks compute.

## Learning
For the RAPIDS Accelerator for Apache Spark, the latest NVIDIA plugin release found was `26.04.2`, and the newest Databricks Runtime listed in NVIDIA's Databricks support matrix was `17.3 ML LTS GPU` with Spark 4.0.0 and Scala 2.13. Databricks has newer runtimes such as 18 and 19 Beta, but they were not listed in NVIDIA's Databricks RAPIDS Spark support matrix. For DBR 17.3, use the `rapids-4-spark_2.13` jar, not the older `_2.12` jar shown in some Databricks getting-started examples. Keep RAPIDS Spark plugin guidance separate from RAPIDS Python/cuDF/cuML notebook installation guidance.

## Future Use
For future Databricks GPU/Spark acceleration work, recommend DBR 17.3 ML LTS GPU, Photon disabled, one-GPU workers, cluster-scoped init scripts, and qualification-tool validation unless newer NVIDIA support docs supersede this.
