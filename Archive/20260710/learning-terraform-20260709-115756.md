---
type: short-term-learning
topic: terraform
created: 2026-07-09 11:57:56
source: codex-conversation
confidence: high
---

# Learning: Lifecycle Guards Must Distinguish Missing Outputs From Bootstrap

## Context
While reviewing `dbx_external_locations/scripts/destroy-guard.sh`, the lifecycle guard treated missing Terraform outputs as an empty first-apply baseline. Local Terraform `v1.14.8` was checked and `terraform output -json` returned `{}` with exit code `0` both before any local state existed and after applying a state with resources but no outputs.

## Learning
For Terragrunt lifecycle guards that enforce `enabled -> destroy_pending -> destroyed -> row removed`, a missing or renamed expected output must fail closed when state exists. Defaulting `(.expected_output // {} | .value // {})` to `{}` can bypass row-removal protection because prior keys disappear from the guard's baseline.

## Future Use
When updating these guards, allow `{}` only for a true bootstrap/no-state condition. If Terraform state exists but the expected lifecycle output is absent or malformed, return a clear guard error and add a regression test for missing output/no-output state.
