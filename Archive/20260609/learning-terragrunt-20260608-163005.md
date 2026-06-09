---
type: short-term-learning
topic: terragrunt
created: 2026-06-08 16:30:05
source: codex-conversation
confidence: high
---

# Learning: sandbox catalog Terragrunt readability posture

## Context
The user asked for multi-agent scoring of the Databricks sandbox catalog catalog unit in the terragrunt catalog repo, focused on looks, readability, YAGNI, KISS, and safe simplification.

## Learning
The reviewed `units/dbx_sandbox_catalogs/terragrunt.hcl` was generally acceptable but not clean, with consensus scoring around 70/100. Its guard hooks and tombstone handling were seen as justified safety complexity, while the main readability problems were duplicated active sandbox filtering, a dense inline `sandbox_catalog_definitions` ternary, surprising `rm -rf tests/` init hook, and implicit team identity group-name convention.

## Future Use
For future review or edits, prefer narrow cleanup over redesign: reuse an `active_sandbox_catalogs` local, move sandbox definition construction into named locals, document or remove the `remove_tests` hook, and explain order-only team identity dependencies. Do not treat the guard JSON/hooks as automatically overengineering; evaluate them as lifecycle safety controls.
