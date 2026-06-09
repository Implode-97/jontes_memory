---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:17:23
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment prd tests should stay contract-sized

## Context
During a read-only scoring review of `databricks-workspace-assignments-wrapper/tests/mock_team_role_nesting_prd_mock_apply.tftest.hcl`, the user asked whether the prd mock apply file protects something the dev sibling does not.

## Learning
The useful distinct coverage is narrow: `ws_prd` entitlement group naming and the `workspace_team_assignments` exposure of `privileged_contributor` for cluster-policy ACL consumers. The broad apply fixture, users-baseline override, and exact `team_role_membership_resources` ID assertions mostly duplicate the dev sibling and preserve diagnostic/test-facing outputs rather than stable downstream contracts.

## Future Use
For future workspace-assignment wrapper reviews, preserve direct checks for real downstream contracts such as `workspace_team_assignments.groups.privileged_contributor.display_name` and prd workspace naming. Prefer collapsing dev/prd duplication into one compact contract test, or convert the prd-specific checks to plan-known assertions, unless a real consumer appears for the diagnostic membership-resource outputs.
