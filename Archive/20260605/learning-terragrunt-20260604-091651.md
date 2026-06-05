---
type: short-term-learning
topic: terragrunt
created: 2026-06-04 09:16:51
source: codex-conversation
confidence: high
---

# Learning: Keep E2E Modularization Free Of Temporary Diagnostics

## Context
During CAV-132831 work on `data-platform-terragrunt-catalog`, the user reviewed the platform E2E modularization MR after the branch had already passed CI with the 300s workspace propagation sleep.

## Learning
For the E2E modularization MR, keep the 300s Terragrunt wait that stabilized the fresh workspace/metastore path, but remove temporary diagnostic and retry scaffolding created while investigating a future sleep replacement. The sandbox AWS diagnostics, apply retry wrapper, and related helper functions should not remain in this refactor because they obscure what the modularization actually changes.

## Future Use
When later replacing the fixed sleep with readiness polling, implement that as a separate, deliberate change with narrow tests and clear ownership. Do not revive old diagnostic helpers by default; create new diagnostics only if they directly support the polling investigation.
