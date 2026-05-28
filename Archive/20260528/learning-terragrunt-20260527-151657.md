---
type: short-term-learning
topic: terragrunt
created: 2026-05-27 15:16:57
source: codex-conversation
confidence: high
---

# Learning: Sandbox Catalog Wrapper Registry Rename

## Context
After the Terraform modules MR for sandbox catalog wrappers was merged to master, the Terragrunt catalog branch needed to consume the renamed module package from the GitLab Terraform module registry.

## Learning
The sandbox catalog Terragrunt unit should reference `aws-databricks-account-catalog-wrapper-sandbox/aws` rather than the old `aws-databricks-catalog-wrapper-sandbox/aws` package. The renamed package reset to version `0.1.0` after publication from the modules master pipeline. The old package can still exist in the registry at higher versions, but it should not be used for the new provider-prefixed naming convention.

## Future Use
When updating Terragrunt catalog units after module renames, verify the exact package name and version from the GitLab Terraform module registry instead of assuming monotonic versions across renamed packages.
