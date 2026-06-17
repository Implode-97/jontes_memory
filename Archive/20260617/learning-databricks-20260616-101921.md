---
type: short-term-learning
topic: databricks
created: 2026-06-16 10:19:21
source: codex-conversation
confidence: high
---

# Learning: RAPIDS notebook project setup

## Context
In `/Users/jnyjc2/code/innovation-week/RAPIDS`, the user asked to bootstrap a local VS Code notebook project with UV and remote Databricks-backed execution.

## Learning
The project is notebook-first, not a package/library. It uses a UV-managed `.venv`, Python `>=3.12,<3.13`, and `databricks-connect==18.2.*` because current Databricks Connect guidance ties package minor versions to the target Databricks Runtime line. `ipykernel` belongs in the dev dependency group, and `pyspark` should not be installed alongside `databricks-connect`. The preferred local auth pattern is a named Databricks CLI OAuth U2M profile, currently documented as `RAPIDS_DEV`, plus either `DATABRICKS_CLUSTER_ID` or `DATABRICKS_SERVERLESS_COMPUTE_ID=auto`.

## Future Use
For future work in this repo, keep investigation notes under `docs/`, preserve the explicit local-vs-remote execution boundary, and re-pin `databricks-connect` if the actual target compute is not DBR/serverless 18.2-compatible.
