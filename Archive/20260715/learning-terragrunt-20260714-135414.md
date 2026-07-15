---
type: short-term-learning
topic: terragrunt
created: 2026-07-14 13:54:14
source: codex-conversation
confidence: high
---

# Learning: MR 32 masks canonical E2E destroy failure with janitor recovery

## Context
Reviewing `data-platform-terragrunt-catalog` MR !32 at `508b081` and pipeline `7969847` showed that the real E2E passes only after its normal root stack destroy fails on the non-empty dev workspace root bucket.

## Learning
The added cleanup code works as leak recovery, but it does not repair or validate the canonical Terragrunt destroy path. It waits for partial root destruction, scans cached Terraform output/state to find the surviving bucket, empties it, and runs the full reverse-order janitor; most janitor destroys are then no-ops. The branch also deleted the direct tests for this parsing logic. This broadens the recovery surface and raw local-state scanning will not survive the planned S3 remote-state E2E unchanged.

## Future Use
Prefer emptying explicitly identified E2E workspace root buckets before root stack destroy, using live Terragrunt/Terraform outputs while state is intact, then run the normal root destroy. Keep reverse-order janitor cleanup only as fallback. If recovery parsing remains, retain focused fixture tests and verify remote-state compatibility.
