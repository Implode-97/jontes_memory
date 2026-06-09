---
type: short-term-learning
topic: terraform
created: 2026-06-09 02:52:19
source: codex-conversation
confidence: high
---

# Learning: Team identity real apply smoke should stay narrow and opt-in

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/tests/valid_apply_real_provider.tftest.hcl`, the user asked for a Databricks account identity and downstream contract lens focused on real-provider value beyond mocks.

## Learning
For this wrapper, a real Databricks account `command = apply` smoke is valuable when it proves actual SCIM group creation, provider-computed group IDs, account group lookup, and group nesting through real `databricks_group_member` resources. Its weakest shape is broad repeated assertions that duplicate the adjacent mock apply test's canonical naming and all-edge nesting checks. The bigger lifecycle concern is keeping the live apply under default `tests/`, because plain `terraform test` and module CI can run it unintentionally.

## Future Use
Prefer narrow real-provider assertions that mocks cannot prove, and recommend an explicit opt-in path or protected filtered CI lane for live apply tests.
