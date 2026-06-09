---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:18:18
source: codex-conversation
confidence: high
---

# Learning: Explain patch-level Terraform and AWS provider floors

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-security-group/version.tf`, the user asked for a new maintainer perspective focused on readability, KISS, YAGNI, maintainability, and whether future maintainers would understand why the floors exist.

## Learning
For reusable Terraform modules in this repo, declaring provider requirements with loose minimum constraints is good and matches official Terraform guidance. The readability gap appears when a module uses a patch-level Terraform floor like `>= 1.14.5` or an exact-looking AWS provider floor like `>= 6.35.1` without documenting which language feature, test behavior, provider resource, or provider bug fix forced that minimum. Sibling modules often use `>= 1.14.0`, so unexplained patch bumps read as magic.

## Future Use
In future IaC reviews, accept simple `version.tf` files as mostly KISS, but penalize unexplained patch-level floors. Prefer aligning to repo baseline or adding a terse comment naming the required feature or bug fix.
