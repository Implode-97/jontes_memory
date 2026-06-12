---
type: short-term-learning
topic: terragrunt
created: 2026-06-11 17:48:29
source: codex-conversation
confidence: high
---

# Learning: CAV-131446 scoring risk is resolver contract

## Context
The user asked for a multi-agent score of the important changed files in the CAV-131446 compute policy catalog MR.

## Learning
Independent scoring agreed that the modular E2E stage files and explicit stack wiring are readable and shippable. The main quality risk is concentrated in `units/dbx_compute_policy/terragrunt.hcl` and its resolver/mock behavior: it resolves sibling `workspace_team_assignments` through a shell/JQ helper and falls back to convention-based mocks even for live commands. Reviewers also flagged duplicated team policy object assembly, incomplete/commented `ws_prd` wiring, and guard tests that validate HCL shape but do not directly test duplicate-key guard behavior.

## Future Use
When continuing CAV-131446, prioritize making live apply fail closed when workspace assignment output is missing, removing placeholder stack comments, adding exact E2E key assertions, and adding focused tests for `policy-instance-guard.sh`. Treat the E2E modularization pattern itself as acceptable.
