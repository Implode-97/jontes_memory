---
type: short-term-learning
topic: terragrunt
created: 2026-06-08 15:03:36
source: codex-conversation
confidence: high
---

# Learning: Avoid sibling cache scraping in stack apply

## Context
During review of `data-platform-terragrunt-catalog` MR !22 for CAV-131446, the compute-policy unit tried to resolve `dbx_workspace` and `dbx_workspace_assignments` outputs by running shell helpers against sibling `.terragrunt-cache` directories during `terragrunt stack run apply`.

## Learning
Scraping sibling generated-unit Terraform caches during the same stack apply is not a reliable Terragrunt dependency mechanism. Terragrunt renders unit configs before the downstream sibling cache or outputs may exist, so `run_cmd` helpers that require those caches can fail before dependency ordering has a chance to apply the upstream unit.

## Future Use
For catalog-unit reviews, flag apply-time `run_cmd` cache lookups for sibling outputs as a correctness risk. Prefer normal Terragrunt dependency contracts, staged stack design, or caller-provided normalized inputs over reading `.terragrunt-cache` as a source of truth.
