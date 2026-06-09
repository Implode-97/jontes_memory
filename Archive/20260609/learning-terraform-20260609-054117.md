---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:41:17
source: codex-conversation
confidence: high
---

# Learning: Databricks workspace catalog grant should keep identity tokens unnormalized

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-catalog-grant/main.tf`, the user asked for a Terraform maintainer/refactor-safety view focused on readability, KISS, YAGNI, validation boundaries, and test/output pressure.

## Learning
For this tiny Databricks grant leaf module, keep the workspace-provider precondition because it documents a real provider-scope requirement and gives a better diagnostic than a likely provider/API failure. Prefer moving whitespace hygiene fully into `variables.tf`: reject surrounding whitespace for `principal` and privileges instead of trimming them inside `databricks_grant`, because resource code should not silently change identity tokens unless that is the explicit contract. Sorting a set-derived privilege list is only justified for deterministic provider arguments and output stability; it should not grow into test-driven shaping.

## Future Use
When reviewing this module again, accept the basic shape with cleanup. Push for direct resource attributes, minimal outputs, narrow tests that prove the intended grant and provider-scope guard, and no extra abstraction around a single grant resource.
