---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:13:37
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper mock apply output test shape

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/tests/mock_apply_outputs.tftest.hcl`, the user wanted a Databricks/AWS workspace bootstrap safety lens while preserving readiness, admin bootstrap, deployer membership, and downstream Terragrunt outputs.

## Learning
The mock apply output test is worth keeping because it cheaply verifies baseline metastore readiness, unsupported detach diagnostics, workspace admin group bootstrap, ADMIN permission assignment, deployer service-principal membership, and important downstream outputs such as workspace URL, role name, bucket name, region, and metastore ID. The main readability debt is duplication with the mock plan validation happy path and assertions that freeze broad internal output objects, especially the whole admin group passthrough.

## Future Use
For later workspace-wrapper test reviews, accept this coverage with cleanup. Prefer narrow assertions or resource/module references over broad public output internals, and add membership target checks only when they improve bootstrap safety without expanding the output API.
