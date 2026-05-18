---
type: short-term-learning
topic: terragrunt
created: 2026-05-18 10:27:49
source: codex-conversation
confidence: high
---

# Learning: avoid Terratest test-structure for simple E2E stage gating

## Context
While diagnosing the `data-platform-terragrunt-catalog` platform E2E CI slowness, the user questioned whether Terratest's `test-structure` import was pulling in too much code for feature opt-in/out stages.

## Learning
Importing `github.com/gruntwork-io/terratest/modules/test-structure` compiles the whole package, not just `RunTestStage`. In Terratest `v1.0.0`, the package includes `save_test_data.go`, which imports heavy optional helpers such as AWS, Kubernetes, Packer, SSH, OPA, and Terraform. For this repo's E2E test, only simple stage gating is needed, so a tiny local helper is preferable to the upstream `test-structure` package.

## Future Use
For future Terragrunt catalog E2E work, preserve feature-based stages but implement them locally unless save/load helpers are genuinely needed. Avoid adding broad Terratest modules that expand the Go dependency graph and slow cold CI compilation.
