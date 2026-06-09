---
type: short-term-learning
topic: terraform
created: 2026-06-08 17:09:36
source: codex-conversation
confidence: high
---

# Learning: Terraform module gitignore should avoid source-looking module paths

## Context
During the multi-agent scoring sweep of `data-platform-terraform-modules`, the repo `.gitignore` was reviewed against Terraform lock-file and ignore guidance.

## Learning
Terraform module repo `.gitignore` files should clearly separate local Terraform artifacts, generated CI files, Python caches, and sensitive local tfvars. Broadly ignoring `.terraform.lock.hcl` is debatable because Terraform docs generally expect dependency lock files for root configurations to be reviewable. Module-specific ignores for `examples/`, `tests/main.tf`, or `Makefile` paths look like hidden source files unless a generator convention or comment explains them.

## Future Use
When editing Terraform module ignore rules, prefer generic safety patterns such as `*.tfstate`, `*.tfstate.*`, `**/tests/real.auto.tfvars`, generated CI artifacts, and Python caches. Avoid stale module-specific source-looking ignores, remove redundant state patterns, and explicitly document any intentional lock-file or generated-test harness exception.
