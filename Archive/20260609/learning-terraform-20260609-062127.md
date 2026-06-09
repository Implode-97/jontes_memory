---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:21:27
source: codex-conversation
confidence: high
---

# Learning: Workspace catalog invalid tests should name preconditions clearly

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-catalog/tests/invalid_inputs_mock_plan.tftest.hcl`, reviewers evaluated Terraform mock-plan validation coverage for the Databricks workspace catalog module.

## Learning
The file scores well when it uses a file-level mock Databricks provider, overrides `databricks_current_config.current.is_account = false`, keeps one valid baseline, and changes one invalid input per `run` with `command = plan` and precise `expect_failures`. For workspace catalog, the provider workspace membership check is a resource lifecycle precondition, not ordinary variable validation, because Databricks automatically binds the provider workspace for isolated catalogs.

## Future Use
When reviewing similar catalog tests, preserve the compact one-invalid-run shape. Name cross-variable/resource guard cases as `fails_precondition`, not `fails_validation`, and add a separate focused `is_account = true` provider-scope precondition test only if the suite is expected to cover all module guards. Avoid broad null/type/URL matrices and real-looking environment fixtures that imply live validation.
