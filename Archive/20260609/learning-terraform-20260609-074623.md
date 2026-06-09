---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:46:23
source: codex-conversation
confidence: high
---

# Learning: Storage credential invalid tests should stay precondition-focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/tests/invalid_inputs_mock_plan.tftest.hcl`, the test was evaluated through a Databricks Unity Catalog safety lens.

## Learning
For the workspace storage credential module, mock-plan invalid-input coverage is strong when it uses a file-level mocked Databricks provider, overrides `databricks_current_config.current.is_account = false`, keeps one valid baseline, and changes one invalid field per run. The provider workspace omitted from non-empty `workspace_ids` case should expect failure on `databricks_storage_credential.this`, because that guard is a resource lifecycle precondition protecting Databricks' automatic provider-workspace binding behavior, not variable validation.

## Future Use
When reviewing similar storage credential tests, preserve the compact one-invalid-run structure and avoid null/type/format matrices that do not protect Unity Catalog safety. Prefer naming resource guard cases as `fails_precondition`, and add a single `is_account = true` precondition run only if no other test covers the workspace-level provider requirement.
