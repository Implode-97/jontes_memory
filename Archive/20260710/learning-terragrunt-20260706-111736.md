---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 11:17:36
source: codex-conversation
confidence: high
---

# Learning: Terragrunt Stack Backend Bootstrap Flag and Masked S3 Create Failures

## Context
While investigating data-platform-terragrunt-catalog MR !31 CI failures, the latest failed E2E job ran Terragrunt v1.1.0 with `terragrunt --non-interactive stack run --backend-bootstrap apply`.

## Learning
Do not change this to `terragrunt stack run -- --backend-bootstrap apply`. In Terragrunt v1.1.0, `stack run` registers normal `run` flags, so `--backend-bootstrap` before the Terraform command is the correct Terragrunt flag. Putting it after `--` makes it an OpenTofu/Terraform argument instead. Also, S3 backend bootstrap can report `error checking access to S3 bucket ... GetObject ... NoSuchBucket` after `CreateBucket` fails, because Terragrunt probes object access and masks the original create error.

## Future Use
For similar CI failures, treat the `NoSuchBucket` as evidence the bucket still does not exist, but inspect IAM/SCP/create-bucket request shape before assuming flag parsing is wrong. Avoid all-unit `backend bootstrap --all` on generated stacks with dependency outputs.
