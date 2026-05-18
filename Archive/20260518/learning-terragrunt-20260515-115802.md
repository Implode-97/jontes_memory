---
type: short-term-learning
topic: terragrunt
created: 2026-05-15 11:58:02
source: codex-conversation
confidence: high
---

# Learning: Terragrunt catalog E2E implementation preferences

## Context
This came from planning and starting implementation of the Terragrunt catalog E2E test refactor for sandbox and platform integration coverage.

## Learning
For this repo, the user wants one shared real E2E lane in CI, not multiple per-stack real jobs. Mandatory setup like copying the example tree, stack generation, root apply, deferred destroy, and stack clean should live in the root test flow, while `test_structure` stages should be named by feature such as `team_identities_assert`, `workspace_assert`, and `catalog_sandbox_assert`. The user does not want V1 to promise local real deploy as a normal workflow, and wants explicit CI mutual exclusion for the real deploy lane.

## Future Use
When extending Terragrunt or IaC testing for this user, prefer feature-based staged E2E structure, keep fast guard checks outside the real deploy lane, avoid over-abstracting helpers too early, and treat local real apply as optional debug work rather than a promised default path.
