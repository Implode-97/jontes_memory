---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:10
source: codex-conversation
confidence: medium
---

# Learning: external storage should preserve platform governance

## Context
A planning session about read-only external locations discussed teams bringing S3 storage from their own AWS accounts into the Databricks platform.

## Learning
The platform goal is to remain the single governance surface for data. If teams connect external S3 storage that they own, the preferred pattern is read-oriented ingestion into managed catalogs rather than letting external storage become an uncontrolled write surface. External locations and volumes should respect ownership, workspace binding, and read-only constraints where the feature is explicitly meant for reference or ingestion. Writes for governed platform data should land in platform-managed catalogs and storage.

## Future Use
When reviewing external-location features, ask whether the design preserves Unity Catalog governance and platform cost/security boundaries. Be cautious with direct write access to team-owned buckets unless the user explicitly changes the governance model.
