---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 09:35:16
source: codex-conversation
confidence: high
---

# Learning: Terragrunt v1 Backend Bootstrap For Catalog E2E

## Context
While moving `data-platform-terragrunt-catalog` platform E2E state to S3 remote state, the unique project bucket name avoided the previous cross-account bucket-policy `AccessDenied`, but the next GitLab run failed with `NoSuchBucket` during Terraform init.

## Learning
With Terragrunt v1.x in this repo, a `remote_state` S3 block and a unique bucket name are not enough for automatic backend creation during `terragrunt stack run apply`. The trace explicitly said to rerun with `--backend-bootstrap`. The E2E base Terragrunt options should set `TG_BACKEND_BOOTSTRAP=true` so stack apply, cleanup, and diagnostic Terragrunt calls can create or update the backend before Terraform init touches S3.

## Future Use
For future catalog E2E remote-state work, preserve `TG_BACKEND_BOOTSTRAP=true` in the Go E2E Terragrunt environment unless the S3 state bucket is pre-provisioned externally. If `NoSuchBucket` appears at Terraform init, check for missing backend bootstrap before changing bucket names or IAM again.
