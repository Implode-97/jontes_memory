---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:06:28
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment validation tests should stay negative and contract-focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_plan_validation.tftest.hcl`, the user asked for a strict KISS/YAGNI Terraform validation-test reviewer.

## Learning
The file is mostly shippable because its concise `expect_failures` runs cover invalid `region_code`, `workspace_target_name`, `workspace_id`, and `team_keys` inputs without apply fixtures or broad output checks. The main score reducer is the happy-path assertion against `output.workspace_context.region_code`, because `workspace_context` is likely an input echo/test-shaped output and weaker than the real downstream `workspace_team_assignments` contract. Missing high-value validation branches include whitespace/empty values and length boundaries, especially for `team_keys` and naming inputs.

## Future Use
For this wrapper, prefer preserving concise negative validation coverage, trimming happy output echoes, and adding only focused edge-case `expect_failures` when they protect actual variable validation branches.
