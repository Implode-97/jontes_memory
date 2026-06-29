---
type: short-term-learning
topic: terraform
created: 2026-06-26 13:54:54
source: codex-conversation
confidence: high
---

# Learning: keep primitive outputs scalar-only

## Context
While cleaning up the Databricks external locations feature, multi-agent review flagged broad primitive module outputs as duplicated contract surface and unnecessary maintenance weight.

## Learning
For this user's Terraform style, primitive modules should expose scalar outputs and narrowly scoped maps such as workspace bindings, not broad composite objects that mirror the resource. If an older wrapper needs a composite public output, compose that object in the wrapper from primitive scalar outputs and include only fields that are part of the wrapper contract. Avoid re-exporting nested provider blocks like `aws_iam_role` when equivalent role/trust metadata is already exposed through a more specific wrapper output.

## Future Use
When implementing or reviewing Terraform modules, prefer scalar primitive outputs. If cleanup removes a broad output, search for wrapper consumers and rewire them to scalar outputs instead of restoring the broad primitive contract.
