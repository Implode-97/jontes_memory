---
type: short-term-learning
topic: databricks
created: 2026-06-11 18:25:22
source: codex-conversation
confidence: high
---

# Learning: Compute Policy Grants Use Contributor Group Inheritance

## Context
During the CAV-131446 compute policy catalog MR, the user clarified that Databricks team identity role groups are nested in the modules repo.

## Learning
The team identity hierarchy is cumulative: `owner` is nested into `privileged_contributor`, which is nested into `contributor`, then into lower roles. For team sandbox Databricks compute policies, granting `CAN_USE` to `grp_team_<team_key>_contributor` is enough for contributor, privileged contributor, and owner members. Repeating ACL grants for `privileged_contributor` and `owner` is redundant and makes Terragrunt wiring noisier.

## Future Use
For catalog-side compute policy work, prefer one team policy per team/profile and one team ACL principal: the canonical contributor group. Treat broader workspace-assignment output fields like `compute_policy_acl_roles` as compatibility metadata unless a future requirement proves that duplicate direct ACL entries are needed.
