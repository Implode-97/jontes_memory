---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:43:58
source: codex-conversation
confidence: high
---

# Learning: Keep catalog grant leaf outputs narrow

## Context
During a read-only review of `data-platform-terraform-modules/modules/databricks-workspace-catalog-grant/outputs.tf`, the user asked for a downstream Terragrunt/module consumer perspective focused on KISS, YAGNI, and whether duplicate scalar/composite outputs help.

## Learning
For the single-principal Databricks catalog grant leaf module, scalar outputs for `catalog_name`, `principal`, and `privileges` are defensible because the sandbox catalog wrapper currently consumes those fields to build its own per-team grant objects. The leaf module's separate `grant` object is weaker: it duplicates the same three resource attributes and has no observed direct consumer, so it mostly broadens the public API and risks becoming test-shaped contract surface.

## Future Use
When reviewing or editing this module, preserve scalar outputs if wrapper consumers still use direct references. Prefer pruning the duplicate leaf-level composite unless a real caller passes the whole grant object downstream.
