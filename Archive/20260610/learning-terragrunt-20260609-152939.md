---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:29:39
source: codex-conversation
confidence: high
---

# Learning: Terragrunt JSON guard wrapper cleanup

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_team_identities/scripts/run-guard-with-json.sh` as a transport wrapper for passing Terragrunt-generated JSON into reusable shell guards.

## Learning
The wrapper shape is justified when Terragrunt hook `execute` arrays pass `jsonencode(...)` as one argument, while guard scripts still own JSON validation and prior-state lifecycle safety. A temp-file bridge is acceptable because the existing guards consume file paths and the input should not become a persisted Terragrunt `generate` artifact. The main readability and maintainability problem is using POSIX `exec` after installing an `EXIT` cleanup trap: successful `exec` replaces the wrapper shell, so the trap does not clean the temp file after the guard exits.

## Future Use
For future reviews of similar guard wrappers, accept the transport-only structure but recommend dropping `exec` or otherwise preserving cleanup. Do not move guard validation into the wrapper unless the wrapper contract intentionally becomes more than transport.
