---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:49:52
source: codex-conversation
confidence: high
---

# Learning: workspace assignment registry guard cleanup lens

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/registry-guard.sh` for shell readability, KISS, YAGNI, and maintainability under a strict reviewer persona.

## Learning
The useful behavior is the registry consistency check: active workspace placement rows (`enabled` and `destroy_pending`) must reference an enabled `_org/teams.yml` team row, and all violations should be reported together. The main readability debt is shell/JQ scaffolding around that small policy: comment-stripping generated JSON is likely YAGNI, top-level key checks claim arrays without validating array shape, `.teams` shape is a hidden generator assumption, and wrapping the jq stream in an array before printing adds noise.

## Future Use
For future workspace-assignment registry guard reviews, preserve the consistency check and all-violations output. Prefer narrow cleanup that either clearly trusts the generated input contract or validates `.teams` with one small jq check; avoid broad shell helpers or duplicated validation unless independent script execution is an explicit requirement.
