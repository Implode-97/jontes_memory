---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:06
source: codex-conversation
confidence: high
---

# Learning: prefer underscore snake case for group keys

## Context
In the sandbox group management planning thread, the user explicitly reversed an earlier naming suggestion and chose underscore snake case for team and group identifiers.

## Learning
For Databricks platform team, group, and registry identifiers, the user prefers underscore snake case because it is easier to work with in IDEs and code. This applies especially to team keys, role group names, and YAML registry fields that are manipulated in Terragrunt, Terraform, and generated tests. Names should be deterministic and human-readable, with role suffixes making the principal purpose obvious.

## Future Use
When proposing naming conventions, do not default to kebab case or mixed punctuation for these IaC-facing identifiers. Use snake_case for stable keys and generated group names unless a downstream provider or existing resource contract requires a different format.
