---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:05
source: codex-conversation
confidence: high
---

# Learning: team identities use nested role groups

## Context
Several Databricks platform sessions and ADR drafting work described the team identities feature and why it exists.

## Learning
The platform is moving from one broad group per team toward a team identity model with nested role groups. The intended role names include `_owner`, `privileged_contributor`, `contributor`, `new_contributor`, `reader`, and `consumer`, giving teams freedom to decide who belongs in each role. The IaC should handle groups that may already exist from SCIM or IdP sync, and may create empty account-level groups when they are not synced yet. Workspace assignment is a separate feature boundary.

## Future Use
When working on team identity, sandbox, cluster policy, or workspace assignment features, preserve account-level identity ownership and role separation. Do not collapse role groups into one team group or assume workspace membership is managed by the identity feature itself.
