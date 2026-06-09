---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:55:02
source: codex-conversation
confidence: high
---

# Learning: Compute policy variable validation cleanup target

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-compute-policy/variables.tf`, the user asked for a Terraform expression/readability maintainer view focused on validation locality, error messages, duplicated logic, and KISS/YAGNI.

## Learning
The `policy_instances` one-map input, strict object type, no-op default, JSON object validation, ACL principal checks, tag checks, and max-cluster validation are worth preserving. The main cleanup target is duplicated implementation logic: effective policy-name generation is repeated in a dense variable validation and again clearly in `main.tf`, while reserved-key detection exists both as variable validation and as a precondition with a more useful violation list. The file also says inputs are normalized while `main.tf` still trims identity-like values.

## Future Use
For future compute policy reviews, recommend accept-with-cleanup rather than broad rework. Prefer moving name-collision checks to a main/precondition path that reuses `local.final_policy_names`, keeping JSON syntax validation in the variable, and choosing one reserved-key diagnostic path. Push for consistent reject-vs-trim behavior for stable identity tokens.
