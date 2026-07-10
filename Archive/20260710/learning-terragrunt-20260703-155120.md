---
type: short-term-learning
topic: terragrunt
created: 2026-07-03 15:51:20
source: codex-conversation
confidence: high
---

# Learning: catalog E2E remote state is S3 lockfile based

## Context
While investigating repeated post-apply EOFs in `data-platform-terragrunt-catalog`, the example root remote state was re-enabled for the platform E2E path.

## Learning
The example root now uses Terragrunt `remote_state` with the S3 backend, generated `backend.tf`, `encrypt = true`, `use_lockfile = true`, tagged bucket creation, and state keys under `data-platform-terragrunt-catalog/non-prod-env`. The older DynamoDB lock-table variable was removed because this path is using Terraform/OpenTofu S3 lockfiles. Terragrunt should auto-create the S3 backend bucket in the deployment AWS account when missing.

## Future Use
When triaging future E2E cleanup or output issues, expect Terraform state to live in S3, not local `terraform.tfstate`. The janitor fallback searches for initialized Terraform cache working directories so direct `terraform output` and `terraform destroy` can use the generated remote backend metadata.
