---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:29:32
source: codex-conversation
confidence: high
---

# Learning: dbx team identity guard wrapper cleanup

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_team_identities/scripts/run-guard-with-json.sh` for looks, readability, KISS, YAGNI, maintainability, and new platform maintainer safety.

## Learning
The tiny wrapper is justified because Terragrunt hooks pass `jsonencode({ teams = local.guard_teams })` as a command argument while sibling guards consume a file path. The `mktemp`, raw `printf`, and wrapper-level temp-file cleanup are the right level of ceremony. The maintainability problem is using `exec sh ...` after installing an `EXIT` cleanup trap: current POSIX shell docs define `exec` as not returning to the shell, so the wrapper trap is not a reliable cleanup path and the README claim that guard input is ephemeral becomes less trustworthy.

## Future Use
For future reviews of shell wrappers around Terragrunt guard JSON, prefer running the child shell normally so `EXIT` cleanup can run. Avoid broader guard refactors when the only issue is preserving temp-file cleanup; keep validation in the sibling guard scripts unless duplicate contract checks become the primary readability problem.
