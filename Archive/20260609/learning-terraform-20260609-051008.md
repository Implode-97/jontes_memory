---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:10:08
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment team-role apply test needs contract-focused cleanup

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_team_role_nesting_dev_mock_apply.tftest.hcl`, the user asked for a downstream Terragrunt/catalog consumer lens.

## Learning
For this wrapper, the useful test value is protecting the stable `workspace_team_assignments` contract consumed by workspace-scoped units such as cluster policy: `assigned_roles`, `compute_policy_acl_roles`, and canonical display names for all five team identity roles. Assertions against `team_role_nesting` and especially exact `team_role_membership_resources.*.resource_id` mock values mostly preserve diagnostic/test-facing outputs and mock plumbing. The dev/prd sibling apply tests share broad fixture shape and can be consolidated or narrowed.

## Future Use
When reviewing or editing these tests, prefer exact contract assertions on `workspace_team_assignments` and avoid broad diagnostic output preservation. Keep apply fixtures only where they prove real group-nesting behavior; otherwise favor smaller plan/contract checks or direct resource relationship assertions.
