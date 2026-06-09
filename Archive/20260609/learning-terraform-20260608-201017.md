---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:10:17
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper outputs should separate contracts from diagnostics

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/outputs.tf`, the user asked for a Databricks/AWS platform safety lens focused on looks, KISS, YAGNI, maintainability, and sensitive output exposure.

## Learning
For the AWS Databricks workspace wrapper, preserve outputs that enable safe downstream workspace use and real bootstrap troubleshooting: workspace ID/name/URL, AWS region/account/resource IDs, metastore readiness, metastore diagnostics, MWS credentials/storage/network IDs, and clearly sensitive generated policy JSON when it is needed to compare Terraform-generated AWS/Databricks bootstrap policy shape during failures. The cleanup target is not the safety diagnostics themselves; it is duplicate aliases and broad internal pass-throughs such as whole admin-group module objects, assignment internals, deployer membership details, and both generic and `mws_*` names for the same IDs.

## Future Use
In future wrapper output reviews, score narrow stable contracts higher than mirror outputs. Keep actionable readiness and troubleshooting outputs, but ask for one canonical name per value and for internal admin/policy debug surfaces to be explicitly minimized, sensitive, or labeled as diagnostic rather than general public API.
