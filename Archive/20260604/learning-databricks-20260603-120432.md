---
type: short-term-learning
topic: databricks
created: 2026-06-03 12:04:32
source: codex-conversation
confidence: high
---

# Learning: Databricks metastore assignment can need post-apply readiness time

## Context
During CAV-132831 Terragrunt E2E investigation, official Databricks docs and KB were checked for why sandbox catalog creation only passed after a fixed wait following fresh workspace and metastore creation.

## Learning
Databricks documents that Terraform-created workspaces need metastore assignment to enable Unity Catalog and identity federation, and an official KB for Terraform dependency errors says permission assignment can race when it runs before metastore assignment completes. The same KB recommends explicit dependency ordering and says adding sleep time after metastore assignment can help with dependency issues. This supports fresh workspace/metastore propagation as a plausible root cause when downstream UC APIs fail immediately after assignment.

## Future Use
For Databricks Terragrunt E2E readiness, prefer replacing fixed sleeps with bounded polling: account metastore assignment GET, workspace current-metastore visibility, and storage credential validation for the exact external-location URL once the credential and IAM role exist.
