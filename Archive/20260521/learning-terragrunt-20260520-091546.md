---
type: short-term-learning
topic: terragrunt
created: 2026-05-20 09:15:46
source: codex-conversation
confidence: high
---

# Learning: guard tests should catch destroy-safety asymmetry before the E2E lane

## Context
This came from discussing why Terragrunt full-stack destroy can fail even when Terraform module smoke tests pass. The user wants the shared E2E lane to expose real integration bugs, but not to be the first place where obvious hook or lifecycle asymmetry is discovered.

## Learning
For this repo, some destroy failures can be identified before live E2E by reading unit Terragrunt code and by adding local guard tests. The main code-review signals are hook asymmetry between `plan`/`apply` and `destroy`, dependency outputs that are still needed to render provider config during destroy, and mock allow-lists that hide real destroy inputs. Fast tests under `tests/terratest/guards` should cover lifecycle guard transitions and any prior-state row-removal rules driven by `terraform output -json`.

## Future Use
When adding or reviewing a Terragrunt unit, do not treat E2E as the first destroy-safety check. Update the docs and add guard coverage whenever a change touches lifecycle hooks, destroy guards, or output-driven row-removal behavior. Use E2E for the full integrated stack proof, not for catching avoidable local asymmetry bugs first.
