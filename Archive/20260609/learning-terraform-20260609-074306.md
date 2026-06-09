---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:43:06
source: codex-conversation
confidence: high
---

# Learning: storage credential outputs should expose bootstrap facts narrowly

## Context
During read-only multi-agent scoring of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/outputs.tf`, reviewers compared the output surface with wrapper consumers and Databricks AWS storage credential bootstrap behavior.

## Learning
For Databricks storage credential modules, `external_id` is a high-value first-class output because it is needed after credential creation to complete the AWS IAM trust policy. `name` is also a real cross-module contract for external locations, and explicit `workspace_bindings` can be useful because Databricks' automatic provider-workspace binding is intentionally unmanaged. The weak pattern is exporting both narrow scalars and a broad mirror object that duplicates provider internals such as full `aws_iam_role`, Terraform/provider IDs, and isolation enums without proven consumers.

## Future Use
When reviewing storage credential output APIs, preserve bootstrap and wiring facts, but push to narrow composite outputs before API freeze. Prefer parent-friendly summaries and exact downstream needs over broad provider-shaped objects or duplicate scalar/composite output surfaces.
