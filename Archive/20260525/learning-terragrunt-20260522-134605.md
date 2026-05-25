---
type: short-term-learning
topic: terragrunt
created: 2026-05-22 13:46:05
source: codex-conversation
confidence: high
---

# Learning: catalog MR updated to corrected wrapper artifacts

## Context
After the modules repo fix for exact Terraform module registry dependency lookup was merged, Codex verified the newly published module artifacts before updating the Terragrunt catalog MR branch CAV-132831.

## Learning
The corrected successful modules master pipeline 7662011 published exact package versions needed by the catalog E2E: databricks-account-metastore-wrapper/databricks 0.4.0, databricks-account-metastore/databricks 0.4.0, aws-databricks-workspace-wrapper/aws 0.5.0, and aws-databricks-workspace/aws 0.5.0. The catalog branch feature/codex/CAV-132831-platform-e2e now has commit fe3f660, which updates units/dbx_metastore to wrapper 0.4.0 and units/dbx_workspace to wrapper 0.5.0.

## Future Use
When continuing MR 21 or debugging its next E2E run, treat these wrapper versions as the intended corrected registry pins. If Terraform init still fails, inspect the newly generated package tarballs and exact child package identities first before investigating Databricks runtime behavior.
