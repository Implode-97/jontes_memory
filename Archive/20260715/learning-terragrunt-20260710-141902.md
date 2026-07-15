---
type: short-term-learning
topic: terragrunt
created: 2026-07-10 14:19:02
source: codex-conversation
confidence: high
---

# Learning: Lifecycle guards must fail closed on state-read errors

## Context
The CAV-134270 data product destroy guard reads `terraform output -json` to compare current YAML rows with previously applied lifecycle states.

## Learning
Treating every failed `terraform output` call as an empty first-apply state defeats the lifecycle guard when the failure is an authorization, backend, network, or corruption problem. It can make an existing resource appear absent and allow a direct destructive transition or premature row removal. A true first apply should be distinguished from other state-read failures using narrow, tested no-state or uninitialized-state diagnostics.

## Future Use
Fail closed for unknown Terraform output errors. Permit an empty prior-state map only for recognized first-apply conditions, following the stricter workspace-assignment guard pattern. Tests should cover both an accepted no-state diagnostic and a rejected generic error such as AccessDenied or a backend transport failure.
