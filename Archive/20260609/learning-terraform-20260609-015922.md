---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:59:22
source: codex-conversation
confidence: high
---

# Learning: Team identity outputs are mostly contract-shaped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/outputs.tf`, the user asked for a pragmatic platform maintainer view before API freeze.

## Learning
`managed_team_keys` and `team_lifecycle_states` are real downstream guard contracts and should stay narrow and explicit. The nested `team_groups` output is also mostly justified because tests and sandbox composition consume canonical owner/contributor group names plus cost metadata as the primary team identity contract. The main cleanup pressure is not the nesting itself, but registry echo fields that may not be needed by real consumers, especially `display_name`, `team_key` inside an object already keyed by team, and possibly `state` if `team_lifecycle_states` remains the lifecycle source of truth.

## Future Use
For future reviews or edits, preserve lifecycle tombstones and one nested role object rather than splitting into many parallel maps. Before freezing the API, prune only fields that are not consumed or clearly documented as part of downstream composition.
