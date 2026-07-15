---
type: short-term-learning
topic: databricks
created: 2026-07-10 14:54:30
source: codex-conversation
confidence: high
---

# Learning: Data product owners must be non-destroyed teams

## Context
This decision was confirmed while correcting the data product catalog feature across the Terraform modules and Terragrunt catalog repositories. Team lifecycle state comes from the organization-level `teams.yml` registry.

## Learning
Every active or `destroy_pending` data product environment must resolve its `owner_team_key` to a team whose state is not `destroyed`. Both `enabled` and `disabled` teams remain valid owners; only a destroyed team is invalid. Destroyed data product tombstones do not require owner metadata because they exist only to complete the guarded removal sequence.

## Future Use
Filter the team lookup map to non-destroyed teams before deriving owner metadata such as cost center and unit name. Add tests that reject destroyed owners and accept disabled owners. Keep this rule explicit in data product documentation and do not silently strengthen it to enabled-only unless the team lifecycle contract changes.
