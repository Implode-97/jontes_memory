---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 19:05:06
source: codex-conversation
confidence: high
---

# Learning: live provider parents need active consumers

## Context
This came from completing the multi-agent score report for all tracked files in the `ng-iac` workspace, especially the final live Terragrunt provider parent and root backend files.

## Learning
Live shared provider parents score well when they have visible active consumers and own one clear generated provider contract, such as the Databricks account provider or AWS assume-role provider. Small provider files score poorly when they are dormant alternate modes, rely on ambient credentials without an explicit use case, or share the same generated output path as the active parent. Active root S3 remote state with `use_lockfile = true` is a good KISS pattern; unused root locals are the main cleanup.

## Future Use
For future live Terragrunt reviews, first prove provider parent consumers with search before judging line count. Prefer one obvious provider mode, keep generated backend/provider ownership explicit, and remove unused compatibility locals instead of adding comments.
