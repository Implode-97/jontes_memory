---
type: short-term-learning
topic: terragrunt
created: 2026-05-18 17:49:03
source: codex-conversation
confidence: high
---

# Learning: live YAML files define platform intent

## Context
Retroactive scan of the sandbox, team identity, cluster policy, and data product planning sessions showed a stable Terragrunt catalog convention.

## Learning
The user prefers live Terragrunt YAML files to describe platform intent in a flat, readable structure rather than deeply nested documents. Use `_org/teams.yml` for stable team metadata, optional role override files only for deviations, region or metastore scoped files for sandbox and data product catalogs, and workspace scoped files for workspace-specific bindings such as policies. Do not silently normalize a missing required top-level key to an empty list; that hides malformed input and undermines guard tests.

## Future Use
When writing Terragrunt units or guards, preserve the distinction between "intentionally empty" and "malformed or missing." Use the folder hierarchy to infer scope, and avoid YAML shapes that make common team additions hard to scan or refactor.
