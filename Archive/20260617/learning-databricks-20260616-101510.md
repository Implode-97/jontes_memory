---
type: short-term-learning
topic: databricks
created: 2026-06-16 10:15:10
source: codex-conversation
confidence: high
---

# Learning: RAPIDS VS Code Databricks workflow

## Context
While investigating current official Databricks and VS Code docs for `/Users/jnyjc2/code/innovation-week/RAPIDS`, a concise report was written to `docs/vscode-databricks-extension.md`.

## Learning
The RAPIDS workspace is a small local Python project using Python 3.12 with `databricks-connect==18.2.1`, `databricks-sdk`, `ipykernel`, pandas, and no standalone `pyspark` package. Databricks CLI `v0.296.0` is available. Current Databricks docs support a VS Code local notebook workflow through the Databricks extension and Databricks Connect: local Python runs locally, Spark/DataFrame work runs on remote compatible Databricks compute, and non-Python notebooks are better treated as remote job runs rather than full interactive VS Code kernels.

## Future Use
For RAPIDS Databricks notebook work, keep local files as source of truth, configure `databricks.yml`/`databricks.env`, authenticate with a Databricks CLI OAuth profile, and match Databricks Connect to target runtime/compute.
