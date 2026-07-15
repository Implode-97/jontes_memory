---
type: short-term-learning
topic: terragrunt
created: 2026-07-10 14:17:43
source: codex-conversation
confidence: high
---

# Learning: Remote-state E2E extraction branch

## Context
The abandoned `feature/codex/external-locations-v1` worktree contained both committed remote-state experiments and six unrelated uncommitted edits after its external-locations feature had been reimplemented and merged to master.

## Learning
The focused remote-state implementation was forward-ported from current `origin/master` into `feature/codex/remote-state-e2e`, commit `be78e75`. Its boundary is limited to the active S3 backend in `example/non-prod-env`, the project-specific E2E bucket and key prefix, S3 lockfiles and bucket tags, backend bootstrap for root apply and destroy, and matching testing documentation. Old no-op/cache diagnostics, explicit `backend bootstrap --all`, external-location changes, and dirty guard-file edits were intentionally excluded.

## Future Use
Continue remote-state E2E work from `feature/codex/remote-state-e2e`. Preserve the abandoned dirty worktree unless explicitly asked to clean it, and keep `--backend-bootstrap` before the Terraform command in `terragrunt stack run` calls.
