---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:06:36
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment validation tests should cover every declared branch

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_plan_validation.tftest.hcl`, the user wanted a Terraform variable validation coverage lens.

## Learning
For this wrapper, a concise mock-provider plan validation suite is the right shape, but it should track the actual `variables.tf` branches: non-empty/trimspace, regex shape, and max length for `region_code` and `workspace_target_name`; positive-only `workspace_id`; and non-empty/lowercase/underscore team keys. A happy assertion against `workspace_context` is weak because that output mostly echoes inputs; `workspace_team_assignments` is the stronger downstream contract.

## Future Use
When reviewing or editing this file, prefer explicit one-cause `expect_failures` runs for high-value validation branches and keep broad output-preservation assertions out of the validation suite unless they exercise `workspace_team_assignments` or another real consumer contract.
