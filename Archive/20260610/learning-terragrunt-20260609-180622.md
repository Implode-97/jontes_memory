---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 18:06:22
source: codex-conversation
confidence: high
---

# Learning: live registry files must match active consumer shape

## Context
This came from multi-agent scoring `data-platform-terragrunt-live/non-prod/eu-central-1/_teams.yml` for YAML data-contract readability and Databricks/Terragrunt live maintainability.

## Learning
Live data files that look like registries but do not match active consumer contracts are worse than neutral placeholders. A regional `_teams.yml` with `team`, display-style `name`, vague `type`, and nested `optional` flags is misleading when catalog units expect a canonical `_org/teams.yml` style `teams` registry with stable `team_key`, lifecycle, cost, and unit metadata. Inactive sandbox or workspace-placement hints inside an unsupported registry shape create future maintenance traps.

## Future Use
When reviewing live YAML registries, first verify the exact filename, root key, and row schema expected by active units. Penalize orphan or future-looking data that implies unsupported workflows. Prefer deleting dormant registries or aligning them fully to the active contract before adding team, workspace, sandbox, budget, or lifecycle data.
