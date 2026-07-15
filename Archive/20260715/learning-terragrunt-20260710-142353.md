---
type: short-term-learning
topic: terragrunt
created: 2026-07-10 14:23:53
source: codex-conversation
confidence: high
---

# Learning: Remote state invalidates local-state cache recovery

## Context
A review of `feature/codex/remote-state-e2e` in `data-platform-terragrunt-catalog` found that the E2E root now generates an S3 backend while the existing cache-recovery helpers still discover Terraform working directories by searching for a root-level `terraform.tfstate`.

## Learning
Terraform non-local backends do not normally persist infrastructure state to a root-level local file; initialized backend configuration instead lives under `.terraform/terraform.tfstate`. Therefore `latestTerraformWorkingDirWithLocalState`, which skips `.terraform` and requires a root-level state file, becomes unusable after enabling S3 state. This disables both the EOF output fallback and the direct-Terraform cleanup janitor.

## Future Use
When enabling remote state in this E2E, update cache recovery to select initialized Terraform cache directories through generated `backend.tf` plus `.terraform/terraform.tfstate` backend metadata, and add a focused regression test. Do not treat the metadata file as infrastructure state.
