---
type: short-term-learning
topic: databricks
created: 2026-06-09 05:39:34
source: codex-conversation
confidence: high
---

# Learning: Document single-principal Unity Catalog grants narrowly

## Context
During read-only scoring of `databricks-workspace-catalog-grant/README.md`, reviewers compared the docs against `databricks_grant`, `databricks_grants`, workspace-provider scope, and Unity Catalog privilege behavior.

## Learning
For a tiny Terraform module built around `databricks_grant`, the valuable documentation boundary is single principal plus single securable. The README should clearly distinguish this from authoritative `databricks_grants` behavior, explain that the module owns the privilege set for that one principal/catalog pair, and keep workspace binding outside the module contract. It should not grow into a Unity Catalog tutorial.

## Future Use
When reviewing Databricks grant modules, preserve concise notes about workspace-level provider scope, caller-owned binding/dependency ordering, and per-principal grant ownership. Add only the caller-impacting privilege semantics, such as `USE_CATALOG` and catalog-level inheritance, while avoiding broad privilege tables unless the module validates or expands privilege values.
