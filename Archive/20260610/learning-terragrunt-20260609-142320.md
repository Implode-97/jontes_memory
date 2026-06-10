---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:23:20
source: codex-conversation
confidence: high
---

# Learning: Name local E2E stage gates literally

## Context
This came from scoring `data-platform-terragrunt-catalog/tests/terratest/e2e/test_structure_stage_helper.go` for readability, KISS, YAGNI, and maintainability.

## Learning
For this repo, a tiny local stage-gating helper is preferred over importing Terratest `test-structure` just to get `SKIP_<stage>` behavior. The helper scores well when it stays small, logs the exact skip variable, and keeps feature-stage call sites explicit. It scores lower when its name implies the upstream `test-structure` package, when simple string concatenation is hidden behind `fmt.Sprintf`, or when the skip semantics are unclear for empty vs non-empty environment variables.

## Future Use
When reviewing or writing E2E stage helpers, prefer literal names such as `runStageUnlessSkipped`, simple string concatenation, and explicit skip semantics. Do not add stage enums, config structs, or the Terratest `test-structure` package unless the code actually needs its broader save/load/copy helpers.
