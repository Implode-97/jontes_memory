---
type: short-term-learning
topic: terragrunt
created: 2026-06-11 23:36:25
source: codex-conversation
confidence: high
---

# Learning: Terragrunt v1.0.8 Does Not Fix Explicit Stack Dependency Interpolation

## Context
During the CAV-131446 compute policy catalog MR, the user asked to upgrade Terragrunt before exploring the stack/autoinclude pattern. A CI commit pinned `TERRAGRUNT_VERSION=1.0.8`, installed that binary in `hclfmt`, guard tests, and platform E2E, then pushed the branch and reviewed pipeline `7783226`.

## Learning
The upgrade worked mechanically: CI logs showed `terragrunt version v1.0.8`, `terragrunt-hclfmt-check` passed, and `terragrunt-guard-tests` passed. Platform E2E still failed at `dbx_compute_policy` with `There is no variable named "dependency"` after `dbx_workspace_assignments` produced `workspace_team_assignments`. Therefore, direct `dependency.*.outputs` interpolation inside this explicit stack-generated compute policy unit is still not viable with Terragrunt v1.0.8.

## Future Use
Do not spend more time expecting a plain Terragrunt patch upgrade to fix this branch. Next steps should explore the stack/autoinclude dependency wiring model or return to a fail-closed materialized contract file for compute policy inputs.
