---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 18:12:22
source: codex-conversation
confidence: high
---

# Learning: workspace provider dependency paths

## Context
This came from multi-agent scoring `data-platform-terragrunt-live/non-prod/eu-central-1/dbcore/_workspace_provider.hcl` for Terragrunt workspace provider contract readability and live dependency safety.

## Learning
`_workspace_provider.hcl` files are small but high-impact because `provider_databricks_workspace.hcl` consumes `workspace_dependency_path` directly as a Terragrunt dependency `config_path`. For generated stack units, paths should point to the actual generated unit, typically including `.terragrunt-stack/dbx_workspace`. A path like `../ws_dev/dbx_workspace` can look readable but miss the generated-unit boundary and break provider resolution. `workspace_target_name` helps humans and CI, but it does not prove the path is correct.

## Future Use
When reviewing workspace provider selectors, check `workspace_target_name`, `workspace_dependency_path`, and mock URL as one contract. Prefer explicit generated-unit paths over inferred or non-generated paths, and treat mismatches between identity and path as rework-worthy even when the file is only a few lines.
