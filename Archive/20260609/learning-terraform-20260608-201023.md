---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:10:23
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper outputs need contract pruning

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/outputs.tf`, the user asked for a new platform maintainer perspective on readability, KISS, YAGNI, and stable output contract shape.

## Learning
For the AWS Databricks workspace wrapper, the output file is understandable but should be treated as slightly below the adjacent README/main scores until duplicate aliases and diagnostic surfaces are pruned. The clearest cleanup targets are duplicate region outputs, generic credential/storage/network aliases that mirror the precise `mws_*` outputs, and whole child-module passthroughs such as the admin group object. Sensitive generated policy JSON outputs are acceptable when marked sensitive and used for tests or debugging, and metastore readiness diagnostics are justified by known Databricks workspace/metastore bootstrap races.

## Future Use
In future reviews of wrapper outputs, separate stable downstream contract from temporary diagnostics before API freeze. Prefer one precise name per value, explicit selected fields over whole module passthroughs, and clear descriptions for outputs kept primarily for troubleshooting.
