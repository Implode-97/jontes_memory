---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:24:40
source: codex-conversation
confidence: high
---

# Learning: Keep Terragrunt Guard Validation Ownership Explicit

## Context
During multi-agent scoring of Databricks team identity Terragrunt shell guards, separate `registry` and `destroy` hooks both validated the same generated JSON shape before enforcing different safety policies.

## Learning
For paired Terragrunt guards over the same generated input, duplicated validation should be a conscious fail-closed ownership decision, not accidental copy/paste. If both hooks must remain independently runnable, keep the duplicated prelude tiny and documented. If they always run together, prefer one canonical shape validator or a narrowly shared helper. Also avoid treating missing Terraform outputs as silently equivalent to a valid first apply unless that distinction is explicit.

## Future Use
When reviewing or implementing Terragrunt lifecycle guards, score repeated jq/shell validation lower unless it clearly protects independent execution. Check whether first-apply bootstrap, missing state, malformed output, and renamed outputs are distinguishable in messages and tests.
