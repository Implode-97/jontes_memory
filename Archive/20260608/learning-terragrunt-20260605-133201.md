---
type: short-term-learning
topic: terragrunt
created: 2026-06-05 13:32:01
source: codex-conversation
confidence: high
---

# Learning: Keep E2E Outputs Terragrunt-First With Isolated Cache Recovery

## Context
In `data-platform-terragrunt-catalog` MR !26 for CAV-132831, the modularized platform E2E test was changed to read generated-unit outputs through `terragrunt output -json`. CI pipeline `7746823` showed this works for most units, but `dbx_sandbox_catalogs` still returned Terragrunt `EOF`.

## Learning
For this shared E2E, normal assertions should stay on the Terragrunt command surface for readability and interface consistency. Direct `.terragrunt-cache` or local `terraform.tfstate` probing is acceptable only as an explicitly named recovery fallback, confined to recovery helper files.

## Future Use
When changing these tests, avoid moving cache/state scanning into the stage assertion files. Prefer Terragrunt output first, then a narrow Terraform cache fallback if Terragrunt fails, and document any fallback use in CI trace review.
