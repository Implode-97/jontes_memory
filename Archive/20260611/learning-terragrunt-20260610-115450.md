---
type: short-term-learning
topic: terragrunt
created: 2026-06-10 11:54:50
source: codex-conversation
confidence: high
---

# Learning: E2E stage ownership for upstream outputs

## Context
During MR !26 modularization of the Terragrunt catalog platform E2E, the user challenged wrapper helpers that repackaged metastore and workspace outputs into local structs before assertions.

## Learning
Each E2E test structure should assert the feature it owns. Metastore output contract checks belong in a dedicated `metastore_assert` stage, not repeated indirectly inside workspace or sandbox stages. Downstream stages may still read upstream outputs as fixture values for their own assertions, but should avoid repackaging those outputs into wrapper structs or re-testing the upstream feature contract.

## Future Use
When adding E2E stages, keep output access direct with `readUnitOutputMap` and `requireOutputValue[T]`. Use helper functions for repeated assertions owned by the current stage, not for rebuilding broad upstream output bundles. Keep nested domain structs only when they make object assertions clearer.
