---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 10:00:50
source: codex-conversation
confidence: high
---

# Learning: Explicit E2E Backend Bootstrap Before Stack Apply

## Context
While moving `data-platform-terragrunt-catalog` platform E2E to S3 remote state, GitLab pipeline `7924432` failed before any real resource apply with `NoSuchBucket` for `traton-dmp-498873881431-data-platform-tg-catalog-e2e-tfstate`.

## Learning
The first failure was missing remote-state bucket creation during initial `terragrunt stack run apply`; later `Backend initialization required`, dependency-output, and `Unknown variable dependency` messages were cleanup fallout from uninitialized generated units. Setting `TG_BACKEND_BOOTSTRAP=true` in Terratest env was not enough evidence that stack execution would create the bucket. The E2E now explicitly runs `terragrunt --non-interactive backend bootstrap --all` after `stack generate`, then runs initial `stack run --backend-bootstrap apply`.

## Future Use
For catalog E2E remote-state failures, treat early `NoSuchBucket` plus Terragrunt's `--backend-bootstrap` suggestion as a backend bootstrap contract problem before investigating Databricks units or cleanup output. Keep bootstrap as an explicit pre-apply step when relying on Terragrunt-managed state infrastructure.
