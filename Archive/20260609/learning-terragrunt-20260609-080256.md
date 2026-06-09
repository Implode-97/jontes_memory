---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 08:02:56
source: codex-conversation
confidence: high
---

# Learning: catalog ignore rules must track generated artifact names

## Context
During read-only scoring of `data-platform-terragrunt-catalog/.gitignore`, reviewers compared ignore patterns against the current detect CI artifacts and Terragrunt-generated paths.

## Learning
For the Terragrunt catalog repo, `.gitignore` should stay explicit about real generated artifacts such as Terraform state/plans, Terragrunt cache and stack output, copied E2E environments, guard JSON, Python caches, and Terratest cache directories. The weak pattern is stale generated artifact names: the file ignored `changed-modules.txt`, while current detect jobs emit `changed-units.txt`, `bump-type.txt`, `unit-versions.json`, and `unit-versions.txt`. Duplicate `.terragrunt-cache` entries also lower scan quality.

## Future Use
When reviewing catalog CI or ignore-file changes, compare ignore entries against actual artifact paths in CI YAML and scripts. Prefer narrow generated-artifact rules, remove duplicates, and call out broad `.terraform.lock.hcl` ignores unless the repo clearly treats lock files as local Terragrunt artifacts.
