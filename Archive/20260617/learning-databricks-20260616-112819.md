---
type: short-term-learning
topic: databricks
created: 2026-06-16 11:28:19
source: codex-conversation
confidence: high
---

# Learning: RAPIDS project targets DBR 17.3 GPU cluster

## Context
In `/Users/jnyjc2/code/innovation-week/RAPIDS`, the user switched from serverless Databricks Connect testing to an NVIDIA RAPIDS Spark pilot on a classic GPU ML cluster.

## Learning
The local project now targets Databricks cluster `jnyjc2_explore_rapids` with cluster ID `0616-082231-shovpy4t`, using the `DEFAULT` Databricks CLI profile. Because the cluster is DBR `17.3.x-scala2.13` with ML runtime and GPU node `g4dn.xlarge`, the UV project is pinned to `databricks-connect==17.3.*`, currently resolving to `17.3.9`. `.env` contains `DATABRICKS_CONFIG_PROFILE=DEFAULT` and `DATABRICKS_CLUSTER_ID=0616-082231-shovpy4t`. A live Databricks Connect test passed, and remote Spark reported `spark.version 4.0.0`, `spark.plugins` including `com.nvidia.spark.SQLPlugin`, and `spark.rapids.sql.enabled true`.

## Future Use
Use this cluster and Connect 17.3 line for RAPIDS experiments. Do not switch back to serverless or Connect 18.2 unless the target compute changes and docs/config are retargeted together.
