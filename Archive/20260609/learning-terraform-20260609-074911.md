---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:49:11
source: codex-conversation
confidence: high
---

# Learning: Storage credential mock apply tests should stay behavior-first

## Context
During a read-only review of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/tests/mock_apply.tftest.hcl`, the user asked for a new maintainer and output-API cleanup lens.

## Learning
For Databricks workspace storage credential mock apply tests, the strongest coverage is resource-level behavior: selected workspace mode creates an isolated storage credential, subtracts the provider workspace from explicit bindings, creates read-write bindings for the remaining selected workspaces, and empty `workspace_ids` keeps the credential open with no explicit bindings. Assertions that mirror `name`, `metastore_id`, `workspace_ids`, `access_mode`, `skip_validation`, or the broad `storage_credential` output object are weaker because they freeze output API candidates already under cleanup pressure.

## Future Use
When reviewing or editing this test family, preserve the provider-workspace binding invariant and open-vs-isolated behavior, but trim output assertions to the narrow consumer contract: `name`, `external_id` when meaningful, and explicit `workspace_bindings` reporting.
