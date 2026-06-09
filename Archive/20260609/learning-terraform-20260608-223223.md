---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:32:23
source: codex-conversation
confidence: high
---

# Learning: Mock Apply Contract Test For Group Membership

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/tests/valid_apply_mock_provider.tftest.hcl`, the user asked for a Terraform maintainability/test-contract lens focused on mock overrides, assertions, KISS, YAGNI, and implementation coupling.

## Learning
For this module, a single mock apply test with one user, one nested group, and one service principal is a good contract test. Exact override addresses keyed by the input strings are acceptable because they match the module's intended `for_each` address contract and make mocked membership IDs deterministic. Preserving `membership_resource_ids` through explicit resource-ID overrides and output assertions is useful, not just snapshot noise, because wrappers consume that output.

## Future Use
Score this pattern in the high 80s when assertions stay focused on public outputs and minimally necessary resource attributes. Recommend only narrow cleanup if direct resource assertions duplicate output assertions or make the test more coupled to resource internals than the contract requires.
