---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:38:29
source: codex-conversation
confidence: high
---

# Learning: score dbx_workspace unit as active but cleanup-worthy

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace/terragrunt.hcl` for new-maintainer readability, KISS, YAGNI, and maintainability.

## Learning
The `dbx_workspace` unit is active and mostly understandable because it keeps live wiring in Terragrunt, delegates detailed AWS/Databricks behavior to the wrapper module, passes a small input set, and has a justified aliased Databricks MWS provider for the wrapper contract. The main readability debt is concentrated in duplicated/unexplained operational scaffolding: an apparently redundant default Databricks account provider alongside the aliased provider, repeated GitLab registry token boilerplate, opaque `remove_tests`, and a broad fixed 300-second post-apply sleep for fresh workspace/metastore propagation.

## Future Use
For future catalog reviews, accept this unit shape with cleanup rather than rework. Prefer preserving the active unit contract while centralizing registry/test-removal boilerplate and replacing broad fixed sleeps with bounded readiness checks when workspace-level Unity Catalog consumers are stabilized.
