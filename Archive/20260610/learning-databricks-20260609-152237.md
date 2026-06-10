---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:22:37
source: codex-conversation
confidence: high
---

# Learning: dbx team identity destroy guard readability

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_team_identities/scripts/destroy_guard.sh` for looks, readability, KISS, YAGNI, maintainability, and new platform maintainer comprehension.

## Learning
The destroy guard's lifecycle checks are justified because they protect Databricks account team identity teardown from direct row removal, enabled-to-destroyed skips, and resurrection after destroyed tombstones. The main readability debt is not the existence of the guard, but the duplicated validation prelude shared with `registry-guard.sh` and the dense nested jq transition matrix. Current official Terraform and Terragrunt docs support using `terraform output -json` inside hooks for this kind of pre-plan/pre-apply guard.

## Future Use
For future reviews of adjacent guard scripts, preserve fail-closed lifecycle protection and structured jq parsing. Prefer narrow cleanup: share or compress repeated validation only if it stays obvious, and make state-machine jq read closer to the README table instead of adding broad shell abstractions.
