---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:26:07
source: codex-conversation
confidence: high
---

# Learning: dbx team identities registry guard readability

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_team_identities/scripts/registry-guard.sh` for looks, readability, KISS, YAGNI, maintainability, and new platform maintainer comprehension.

## Learning
The registry guard is acceptable with cleanup because its empty-registry branch protects a real destructive lifecycle invariant: final team row removal is allowed only after Terraform state reports every prior team as `destroyed`. The guard's fail-closed `terraform output -json` read and explicit prior-state checks are justified. The main readability debt is smaller than the sibling destroy guard: comment-stripping generated JSON is likely YAGNI, validation duplicates `destroy_guard.sh`, repeated jq parsing creates shell noise, and the `terraform` binary should ideally come from Terragrunt hook context via `TG_CTX_TF_PATH`.

## Future Use
For future Databricks team-identity guard reviews, preserve the empty-registry safety check and structured jq parsing. Prefer narrow simplifications that remove unsupported input flexibility and reduce duplicate validation without hiding lifecycle policy behind a broad shell helper.
