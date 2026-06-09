---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:32:50
source: codex-conversation
confidence: high
---

# Learning: Score mock apply group membership tests strictly

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/tests/valid_apply_mock_provider.tftest.hcl`, the user asked for a strict KISS/YAGNI Terraform test reviewer perspective.

## Learning
For this module, a mock apply test with one user, one nested group, and one service principal is a reasonable way to cover all three explicit data-source/resource chains without live Databricks risk. Score it as shippable but penalize broad override and assertion volume when many checks only restate mocked data or resource IDs. Preserve assertions that protect public downstream contracts such as `member_group_ids` and `membership_resource_ids.groups`, and value direct resource wiring checks more than raw mocked output equality.

## Future Use
In future reviews of this file or similar Databricks account membership tests, recommend narrow cleanup instead of redesign: remove unused mocked attributes, consolidate repetitive membership resource ID assertions, and keep the fixture explicit rather than adding abstraction.
