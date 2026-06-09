---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:13:38
source: codex-conversation
confidence: high
---

# Learning: mock apply output test scoring

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/tests/mock_apply_outputs.tftest.hcl`, the user asked for a capable new platform maintainer perspective focused on readability, KISS, YAGNI, maintainability, fixture repetition, escaped JSON, assertion naming, and overlap with `mock_plan_validation.tftest.hcl`.

## Learning
For this wrapper, a mock apply output test is useful when it proves apply-only output materialization, Databricks metastore readiness diagnostics, and workspace admin bootstrap behavior. Its maintainability score should drop when the file mostly duplicates plan-test mocks and public-output assertions, because that blurs behavior coverage with broad output API preservation. Escaped one-line JSON fixtures are a readability smell when the JSON policy shape is not itself under test.

## Future Use
In future AWS Databricks workspace wrapper scoring, prefer focused apply assertions over repeating the plan happy path. Recommend `jsonencode` or clearer fixtures for mock policy JSON, and treat duplicated output-snapshot assertions as cleanup unless they protect a real downstream contract.
