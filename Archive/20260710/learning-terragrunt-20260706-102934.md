---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 10:29:34
source: codex-conversation
confidence: high
---

# Learning: Terragrunt v1 Stack Backend Bootstrap Flag Placement

## Context
While triaging `data-platform-terragrunt-catalog` MR !31 pipeline `7924863`, `platform-e2e-test` job `35984713` failed on commit `a0802d2` after moving E2E state to an S3 remote backend.

## Learning
The job ran `terragrunt --non-interactive stack run --backend-bootstrap apply`, but Terragrunt v1.0.0 still failed with `NoSuchBucket` and suggested rerunning with `--backend-bootstrap`. For `stack run`, flags after the stack delimiter are interpreted by the inner `run --all`; parser checks showed `terragrunt stack run -- --backend-bootstrap ...` is the shape that reaches the inner run options. The earlier `terragrunt backend bootstrap --all` approach remains unsafe because it evaluates downstream generated units before dependency outputs exist.

## Future Use
For this repo's E2E stack, prefer `terragrunt stack run -- --backend-bootstrap apply` and equivalent destroy cleanup over all-unit backend bootstrap sweeps. If a separate bootstrap is unavoidable, scope it to dependency-free generated units only.
