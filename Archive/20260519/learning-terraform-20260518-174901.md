---
type: short-term-learning
topic: terraform
created: 2026-05-18 17:49:01
source: codex-conversation
confidence: high
---

# Learning: keep lower modules generic and wrappers platform-specific

## Context
Retroactive scan of the early Terraform module sessions showed the user repeatedly separating generic AWS modules from Databricks workspace wrapper logic.

## Learning
In the Terraform modules repo, lower-level modules such as secure S3 bucket, IAM role, and security group modules should stay generic enough for non-Databricks usage. Databricks-specific policy documents, descriptions, pass-role defaults, workspace wiring, and root-storage semantics belong in the Databricks wrapper module that composes them. If a lower module has hard-coded text like "Databricks workspace bucket" or Databricks-specific assumptions, move that specificity into the wrapper unless the module's feature contract truly is Databricks-specific.

## Future Use
When implementing or reviewing module changes, check the ownership boundary first. Keep reusable AWS resource modules plain and readable; put platform use-case language, generated policy documents, and composition decisions in wrapper modules or Terragrunt.
