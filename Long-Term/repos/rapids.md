---
scope: repo
repo: /Users/jnyjc2/code/innovation-week/RAPIDS
topics: [rapids, cudf-spark, gpu, databricks-connect, notebooks]
stability: verify-current
last-reviewed: 2026-07-10
---

# RAPIDS Notebook Project

- Treat the repository as a notebook-first local VS Code/UV project, not a package. Local files are source of truth and investigations live under `docs/`.
- Keep the local-versus-remote execution boundary explicit.
- June 2026 target: classic GPU ML cluster `jnyjc2_explore_rapids`, cluster ID `0616-082231-shovpy4t`, profile `DEFAULT`, DBR `17.3.x-scala2.13`, Spark `4.0.0`, `g4dn.xlarge`, and Databricks Connect `17.3.*`.
- Use Python `>=3.12,<3.13`, keep `ipykernel` in dev dependencies, and do not install standalone `pyspark` beside `databricks-connect`.
- Keep `.env` aligned with `DATABRICKS_CONFIG_PROFILE=DEFAULT` and the selected cluster ID.
- Use `rapids-4-spark_2.13` for the DBR 17.3 target and keep Spark RAPIDS separate from Python cuDF/cuML notebook setup.
- Prefer Photon disabled, one-GPU workers, cluster init scripts, and RAPIDS qualification tooling for initial pilots.
- `spark-rapids` documentation now routes through `cudf-spark`; treat the artifact and `com.nvidia.spark.SQLPlugin` contract as the same plugin unless NVIDIA documents a split.

## Current State — Verify

- Re-check cluster existence, DBR, Connect, Python, node type, profile, and `.env` before execution.
- Re-check NVIDIA's Databricks support matrix before recommending a newer DBR or RAPIDS artifact; June 2026 support topped out at RAPIDS Accelerator `26.04.2` on DBR 17.3 ML LTS GPU.
