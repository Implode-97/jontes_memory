---
type: short-term-learning
topic: databricks
created: 2026-06-03 12:05:35
source: codex-conversation
confidence: high
---

# Learning: sandbox E2E delay points to UC readiness boundary

## Context
After CAV-132831 platform E2E passed with a hard-coded 900-second pre-sandbox Terragrunt wait, the user asked for a multi-agent root-cause review of what the delay is masking and where the real fix should live.

## Learning
The current evidence points to a fresh Databricks workspace/metastore Unity Catalog readiness gap after Terraform-visible `databricks_metastore_assignment` completion, not a missing Terragrunt dependency edge. The sandbox wrapper already handles its own storage credential, IAM role, and 30-second IAM propagation path, and its real apply test passes against stable upstream resources. The fixed sleep should remain diagnostic, not become permanent architecture.

## Future Use
For future catalog E2E fixes, prefer a Terragrunt-owned workspace UC readiness gate between `dbx_workspace`/`dbx_metastore` and all workspace-level UC consumers. Poll account metastore assignment, workspace current-metastore visibility, and then storage-credential validation where applicable. If evidence narrows failures to per-credential IAM/storage validation, keep the retry in the sandbox/external-location module boundary instead.
