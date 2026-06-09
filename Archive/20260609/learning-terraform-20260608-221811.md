---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:18:11
source: codex-conversation
confidence: high
---

# Learning: AWS Security Group Version Contract

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-security-group/version.tf`, the user asked for a Terraform module consistency perspective focused on readability, KISS, YAGNI, maintainability, and avoiding over-specific version contracts.

## Learning
For this lower-level security group module, `required_version = ">= 1.14.5"` is weaker than the repo pattern because almost all sibling modules use `>= 1.14.0` and the module uses ordinary Terraform constructs. The AWS provider floor `>= 6.35.1` is only justified if it is the actual tested minimum or tied to a provider fix; otherwise prefer the lowest tested common floor, such as the `>= 6.31` used by sibling AWS modules.

## Future Use
When reviewing module `version.tf` files, score arbitrary patch-level bumps down unless the code or tests prove the minimum. Keep provider constraints minimal and tested, and avoid inline rationale comments unless a non-obvious provider bug or schema dependency forces the floor.
