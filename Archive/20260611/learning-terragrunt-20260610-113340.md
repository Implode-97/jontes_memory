---
type: short-term-learning
topic: terragrunt
created: 2026-06-10 11:33:40
source: codex-conversation
confidence: high
---

# Learning: Hybrid E2E output decoding pattern

## Context
While refactoring MR !26 for the Terragrunt catalog platform E2E test, the user questioned whether predefining full Terraform output structs added maintenance when unrelated outputs change.

## Learning
For E2E Terragrunt output assertions, prefer a hybrid pattern: read top-level `terragrunt output -json` through Terratest into a `map[string]json.RawMessage` output envelope, then decode only asserted keys with a small typed helper such as `requireOutputValue[T]`. Keep nested structs for domain objects that are asserted heavily, such as sandbox catalog and grant objects, because they preserve readability. Avoid fully dynamic `map[string]any` assertions.

## Future Use
When adding E2E assertions in this MR, do not recreate large top-level output mirror structs. Add direct typed output extraction for asserted keys, keep cache fallback recovery separate, and add explicit nested `require.Contains` checks when missing-map-key diagnostics matter.
