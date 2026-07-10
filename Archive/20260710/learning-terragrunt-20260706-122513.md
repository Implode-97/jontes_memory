---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 12:25:13
source: codex-conversation
confidence: high
---

# Learning: Unsigned Guard JSON Should Use Plain Overwrite

## Context
While fixing `data-platform-terragrunt-catalog` CAV-133911 external-location E2E lifecycle/no-op failures, the user isolated the Terragrunt generator behavior for guard JSON files consumed by `jq`.

## Learning
For generated guard input JSON files that set `disable_signature = true`, `if_exists = "overwrite_terragrunt"` is the wrong regeneration mode. Repeated applies can fail with a bare EOF because Terragrunt expects ownership metadata that the unsigned JSON intentionally does not have. Adding a Terragrunt signature avoids the ownership failure but makes the files invalid JSON for `jq`, so it is not viable. The correct pattern is `disable_signature = true` with `if_exists = "overwrite"` for these guard JSON generators.

## Future Use
When adding Terragrunt-generated JSON that shell guards parse directly with `jq`, keep the generator unsigned and use plain overwrite. Preserve `overwrite_terragrunt` for signed provider/backend/Terraform files where Terragrunt ownership checks are useful.
