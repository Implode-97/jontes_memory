---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:25:42
source: codex-conversation
confidence: high
---

# Learning: explain cross-workspace selectors

## Context
This came from multi-agent scoring of `data-platform-terragrunt-catalog/example/non-prod-env/eu-central-1/ws_dev/_workspace_provider.hcl`, where the file under `ws_dev` selected `workspace_target_name = "ws_prd"` and pointed at the sibling `ws_prd/dbx_workspace`.

## Learning
Terragrunt workspace-provider selector files should not silently cross workspace boundaries when nearby YAML, stack files, or future units would appear to belong to the local workspace. A selector that intentionally makes `ws_dev` consumers target `ws_prd` can be valid, but it must be locally obvious or proven by an active consumer; otherwise it creates a hidden contract and can make future `workspace_teams.yml` or workspace-assignment changes apply to the wrong workspace.

## Future Use
When reviewing or writing `_workspace_provider.hcl` selectors, check the folder name, selected `workspace_target_name`, dependency path, active stack units, and colocated YAML together. Penalize or simplify cross-target selectors unless they are documented, colocated under the target owner, or currently consumed in a way that makes the boundary unmistakable.
