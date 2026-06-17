---
type: short-term-learning
topic: databricks
created: 2026-06-16 10:15:22
source: codex-conversation
confidence: high
---

# Learning: RAPIDS Databricks Connect dependency posture

## Context
During the RAPIDS innovation-week project investigation, current official Databricks docs were checked for Databricks Connect compatibility and a concise report was written to `docs/databricks-connect.md`.

## Learning
For local VS Code/Jupyter Databricks Connect work in RAPIDS, avoid open-ended `databricks-connect` lower bounds. Pin the package to the target Databricks Runtime minor line using Databricks' `X.Y.*` pattern. As of 2026-06-16, official docs list Python 3.12 for DBR 18.x classic clusters and serverless environment v5, with latest Python Connect 18.2.1 for DBR 18.2 classic and serverless v5 documented against Connect 18.0. The current project `pyproject.toml` uses `requires-python = ">=3.12"` and `databricks-connect>=18.2.1`, which should be tightened before stable use.

## Future Use
When modifying RAPIDS dependencies, prefer `requires-python = ">=3.12,<3.13"`, do not install `pyspark` beside `databricks-connect`, and choose `databricks-connect==18.2.*` for DBR 18.2 classic or the documented serverless-compatible line after running `databricks-connect test`.
