---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:07
source: codex-conversation
confidence: high
---

# Learning: sandbox catalogs are team-owned dev workspaces

## Context
The sandbox catalog design sessions established the intended behavior for team sandbox catalogs in the Databricks platform.

## Learning
Each team should normally get one sandbox catalog in the dev workspace. Additional per-region sandboxes may be allowed when transfer costs or regional GPU constraints justify them, but that is not the default. A sandbox catalog is built from S3 root storage, storage credential, external location, and catalog root configuration. It should not grant other teams access by default. The owning team role should be able to create and manage schemas and tables, while individual object owners can decide how broadly to share child objects within the team.

## Future Use
When implementing sandbox catalogs, treat them as dev-workspace collaboration areas, not productionized data products. Keep region-specific sandbox declarations in that region's YAML and avoid cross-region entries unless the use case explicitly requires it.
