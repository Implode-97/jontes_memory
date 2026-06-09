---
type: short-term-learning
topic: terraform
created: 2026-06-09 02:09:59
source: codex-conversation
confidence: high
---

# Learning: Team identity mock apply test is fixture-heavy

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/tests/valid_apply_mock_provider.tftest.hcl`, the user asked for a strict KISS/YAGNI Terraform test review.

## Learning
The test's mock-provider apply shape is directionally useful because the wrapper creates canonical Databricks team-role groups and group-to-group nesting edges without touching live infrastructure. The score should drop when deterministic `override_resource` and `override_data` blocks duplicate the same role IDs and display names across many target addresses while the assertions mostly need non-empty IDs, canonical display names, or edge existence. Exact membership ID assertions are less valuable than proving each canonical nesting edge exists.

## Future Use
For future reviews or edits of this wrapper's mock tests, preserve apply-mode mock coverage for the role chain, but prefer generated mock values and compact non-empty assertions unless exact forwarding of a public output is the behavior under test.
