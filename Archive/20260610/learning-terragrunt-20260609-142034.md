---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:20:34
source: codex-conversation
confidence: high
---

# Learning: Keep shared E2E proof separate from recovery machinery

## Context
This came from scoring `data-platform-terragrunt-catalog/tests/terratest/e2e/platform_e2e_test.go` for readability, KISS, YAGNI, maintainability, and shared live E2E safety.

## Learning
For shared Terragrunt E2E tests, cleanup reliability is valuable enough to justify root destroy plus reverse-order generated-unit janitors and preserving the copied working tree on failure. However, active assertions should remain easy to distinguish from deferred lifecycle coverage, diagnostics, and recovery helpers. Commented-out lifecycle matrices, normal output reads that scan `.terragrunt-cache`, broad diagnostics on unrelated apply failures, and misleading cleanup flag names all reduce readability even when the underlying safety intent is good.

## Future Use
When reviewing or changing shared E2E code, keep the main path as setup, apply, active assertions, and cleanup. Put cache/state scanning behind explicitly named recovery or janitor helpers, gate expensive diagnostics to known failure signatures, remove dormant commented test blocks until they are active, and label cleanup state by what must happen rather than by what already succeeded.
