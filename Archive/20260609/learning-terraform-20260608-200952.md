---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:09:52
source: codex-conversation
confidence: high
---

# Learning: Keep workspace wrapper outputs narrow

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/outputs.tf`, the user asked for a strict KISS/YAGNI Terraform output API review.

## Learning
The workspace wrapper output file is readable Terraform mechanically, but its public API is broader than current downstream use justifies. Preserve realistic Terragrunt contracts such as `workspace_id`, `workspace_url`, workspace and metastore identity fields, `metastore_attachment_ready`, and core AWS resource IDs/names/ARNs. Challenge duplicate aliases for MWS IDs, whole child-module passthrough outputs, admin bootstrap object internals, generated sensitive policy JSON, and debug-only pass-role lists when usage is only README/tests.

## Future Use
For later reviews of this wrapper, score output breadth as a contract-design issue. Tests can assert internals through resources or narrower outputs, but test convenience alone should not freeze broad wrapper outputs before live callers need them.
