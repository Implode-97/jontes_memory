---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:10:44
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper output contract

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/outputs.tf`, the user asked for a Terragrunt/downstream dependency contract review focused on readability, KISS, YAGNI, and maintainability.

## Learning
The useful wrapper outputs are the stable downstream contracts: `workspace_id`, `workspace_url`, one clear AWS region output, AWS account ID when provider context is useful, metastore attachment/readiness facts, root bucket/role/security group identifiers, and narrow admin group identity. Broad passthroughs are weak API: duplicate MWS aliases, whole child-module objects, generated policy JSON, effective pass-role lists, and deployer membership internals are mostly diagnostics or tests rather than downstream contracts.

## Future Use
For future workspace wrapper reviews or edits, preserve outputs needed by workspace-scoped providers and Terragrunt dependency wiring, but trim mirrored child internals before go-live. If diagnostics are kept, name them explicitly as diagnostics and avoid making test-convenience shapes part of the long-term module API.
