---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:16:23
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper plan validation should stay focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/tests/mock_plan_validation.tftest.hcl`, the user asked for a strict KISS/YAGNI Terraform validation-test perspective.

## Learning
This plan-validation file is serviceable but should score only around the low 70s when judged for readability and maintainability. Its high-value coverage is dangerous naming and dependency validation: workspace-name shape, derived root bucket/role names, root bucket names requiring `root`, and required/UUID-shaped metastore dependency IDs. The weaker parts are duplicated mock setup shared with the mock apply test, escaped JSON fixtures, repeated baseline variables in each negative run, and happy-path assertions that mostly freeze mocked output echoes or broad output API shapes.

## Future Use
For future reviews of this wrapper, recommend accepting with cleanup rather than reworking from scratch. Keep focused plan validation, but prefer top-level baseline test variables, fewer output snapshot assertions, and no broad sweep of low-value scalar validation tests unless the input protects a real naming, dependency, or bootstrap failure mode.
