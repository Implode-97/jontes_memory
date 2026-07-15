---
type: short-term-learning
topic: architecture
created: 2026-07-14 14:58:41
source: codex-conversation
confidence: high
---

# Learning: Workspace destruction is an E2E-only lifecycle

## Context
While reviewing data-platform-terragrunt-catalog MR !32 cleanup, the user clarified that normal platform operation is not intended to destroy Databricks workspaces. Full workspace teardown exists to clean up disposable E2E environments.

## Learning
Do not weaken reusable production workspace or secure-bucket modules merely to make the E2E teardown convenient. In particular, avoid broadly exposing or enabling S3 `force_destroy` when an accidental workspace destroy should remain blocked by non-empty root storage. Put destructive flexibility at the E2E orchestration boundary instead: quiesce E2E compute, resolve explicitly known root bucket outputs while state is intact, verify strong E2E identity such as prefix and run tags, empty those buckets, then require the canonical Terragrunt stack destroy to succeed.

## Future Use
Treat pre-destroy bucket draining as an explicit E2E precondition, not janitor recovery. A failed drain or root destroy must keep CI red; janitor may prevent leaks but must never convert the failed canonical lifecycle into a passing result.
