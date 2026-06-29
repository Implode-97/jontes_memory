---
type: short-term-learning
topic: terraform
created: 2026-06-26 13:23:40
source: codex-conversation
confidence: high
---

# Learning: Score IaC lower for broad duplicate surfaces

## Context
A multi-agent scoring pass over the sandbox catalog external-location feature found a consistent readability issue across Terraform and Terragrunt files: useful safety behavior was present, but some public outputs, generated guard shapes, and test fixtures repeated the same contracts.

## Learning
For this user's IaC work, broad scalar-plus-composite Terraform output surfaces and repeated generated guard/test JSON shapes should score lower on KISS/YAGNI even when functionally correct. Prefer one canonical public contract per consumer need. Keep safety guards, lifecycle checks, and boundary validation, but avoid mirroring internal resource state or caller taxonomy unless downstream automation actually consumes it.

## Future Use
When reviewing or implementing Terraform/Terragrunt modules, look for duplicate outputs, duplicated validation across Terraform and guard scripts, and inline JSON fixture repetition. Suggest fixture builders, narrower outputs, or a single generated shape before accepting more cases into a verbose pattern.
