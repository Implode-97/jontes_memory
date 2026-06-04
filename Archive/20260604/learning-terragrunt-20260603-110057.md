---
type: short-term-learning
topic: terragrunt
created: 2026-06-03 11:00:57
source: codex-conversation
confidence: high
---

# Learning: platform E2E scope after CAV-132831 rebase

## Context
During the CAV-132831 Terragrunt catalog E2E branch review, the user clarified which master changes should not be adopted into the current shared E2E scope after rebase.

## Learning
`wavebreakers` is the canonical team name and assertions/docs/fixtures should align to it, including derived naming such as `cc-wavebreakers`. Workspace assignments and account budget policy should be deferred out of the current shared E2E apply and CI test surface; they can be brought back in a later revision. Old stack tests should remain omitted because the intended migration is a hot-swap to the new shared `platform-e2e-test`, not parallel old stack-test coverage.

## Future Use
When continuing this branch, keep the E2E focused on team identities, workspace, and sandbox catalog smoke coverage. Do not reintroduce live mock locals, workspace assignment units/jobs, budget policy units, or old stack-test jobs unless the user explicitly expands scope.
