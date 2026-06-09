---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:44:07
source: codex-conversation
confidence: high
---

# Learning: Workspace catalog grant outputs should be one shape

## Context
During a read-only KISS/YAGNI output API review of `data-platform-terraform-modules/modules/databricks-workspace-catalog-grant/outputs.tf`, the user asked for a strict Terraform reviewer lens and no local test execution.

## Learning
For this tiny leaf module that owns one `databricks_grant` for one catalog/principal pair, separate scalar outputs plus a `grant` object are duplicate public API. The values are readable and can reflect normalized resource attributes, but they mostly echo the module inputs and are currently preserved by mock output assertions rather than known downstream callers.

## Future Use
Prefer one output shape before API freeze. Keep a single `grant` object only if wrappers need to aggregate catalog grant metadata; otherwise keep the minimal scalar contract callers actually consume. Move tests toward direct resource assertions or one kept output shape, not broad output mirroring.
