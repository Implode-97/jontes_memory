---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 10:12:22
source: codex-conversation
confidence: high
---

# Learning: Do Not Pre-Bootstrap Catalog E2E With Backend Bootstrap All

## Context
In `data-platform-terragrunt-catalog` MR !31, commit `c172fd6` added `terragrunt backend bootstrap --all` after `stack generate` to address missing S3 remote-state bucket creation. Pipeline `7924700` showed this was the new first failing command.

## Learning
`terragrunt backend bootstrap --all` is not a safe pre-apply step for this generated stack. It evaluates all generated units before initial apply, including downstream units whose generated providers and inputs reference `dependency.*.outputs`. Before upstream state exists, this causes `Backend initialization required`, `There is no variable named "dependency"`, and missing bucket errors before the real `stack run apply` can execute. This supersedes the earlier idea that an explicit all-unit backend bootstrap pre-step was the correct fix.

## Future Use
For this E2E stack, do not add all-unit pre-bootstrap sweeps before first apply. Prefer the normal apply DAG with an explicit `terragrunt stack run --backend-bootstrap apply`, or if a separate bootstrap is unavoidable, scope it narrowly to dependency-free root units and prove it does not evaluate downstream dependency outputs.
