---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 10:39:15
source: codex-conversation
confidence: high
---

# Learning: Signature-disabled generated JSON causes Terragrunt EOF

## Context
While reducing repeated post-apply EOFs in `data-platform-terragrunt-catalog`, a local mock fixture reproduced the failure without AWS, Databricks, registry modules, remote state, or real providers.

## Learning
The EOF was caused by generated JSON guard files using `disable_signature = true` together with `if_exists = "overwrite_terragrunt"`. On the first run, Terragrunt writes unsigned one-line JSON with no trailing newline. On the next run, `overwrite_terragrunt` makes Terragrunt check the existing file for its generated-file signature; the signature reader hits EOF and fails before hooks or Terraform run.

## Future Use
For generated files that must remain valid JSON or otherwise cannot carry Terragrunt's signature, use `if_exists = "overwrite"` instead of `overwrite_terragrunt`. Keep `overwrite_terragrunt` only for generated files that preserve Terragrunt's signature comment.
