---
type: short-term-learning
topic: terragrunt
created: 2026-06-02 08:55:08
source: codex-conversation
confidence: high
---

# Learning: Terragrunt E2E Cleanup Needs UC Preflight

## Context
During the CAV-132831 sandbox catalog E2E branch, the latest failed GitLab E2E job left Databricks Unity Catalog resources after catalog destroy failed on missing `MANAGE` and `USE CATALOG` privileges. The example Terragrunt `remote_state` block is commented, so CI uses local state that disappears with the job workspace.

## Learning
The sandbox E2E uses stable catalog names such as `sandbox_firecrackers`, even though AWS naming is made unique through the copied example `naming_prefix`. If cleanup fails, a later E2E run cannot rely on Terraform state to adopt or destroy the leaked catalog, and may fail on an existing Databricks catalog or storage credential before proving a module fix.

## Future Use
Before rerunning shared catalog E2E after a failed cleanup, check for leaked `sandbox_*` Unity Catalog resources and clean them with a principal that can manage the metastore/catalog. Treat this as an environment preflight, not just a code retry.
