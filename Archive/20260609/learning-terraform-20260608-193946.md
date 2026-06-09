---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:39:46
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper output contracts

## Context
During the multi-agent scoring sweep, `aws-databricks-workspace-catalog-wrapper-sandbox/outputs.tf` was reviewed for readability, KISS, YAGNI, and safety-aware simplification.

## Learning
For Terraform wrapper modules, distinguish stable automation contracts from diagnostic or test-convenience outputs before the API settles. In the sandbox wrapper, `sandbox_lifecycle_states` is a real Terragrunt destroy-guard contract and should stay explicit. Workspace binding visibility is also justified because Databricks automatically binds isolated securables to the creation workspace. The recurring issue is broad mirror outputs: duplicating grants and bindings across multiple shapes, forwarding whole child-module objects such as storage credentials, and exposing input echoes or provider internals as if they were stable public API.

## Future Use
When designing or reviewing Terraform wrapper outputs, keep guard/automation outputs narrow and named by purpose. Prefer one clear shape for grants and bindings, expose selected child fields rather than whole child objects, and label operational/debug output separately from long-term module contracts.
