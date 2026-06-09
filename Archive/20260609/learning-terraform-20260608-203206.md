---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:32:06
source: codex-conversation
confidence: high
---

# Learning: Match Wrapper Pass-Through Input Shapes

## Context
While scoring the Databricks workspace wrapper variable contract, a wrapper input for extra security-group egress rules was compared against the child security-group module it forwards into.

## Learning
Wrapper modules should not invent a different object shape for values that are passed straight through to a child module. If the child expects `description` and applies CIDRs from a shared `internet_egress_cidr_blocks` list, a wrapper input using `desc` and per-rule `cidr` is misleading and can silently lose caller intent through Terraform object conversion. Pass-through inputs should either match the child contract exactly or be transformed explicitly with a clear local.

## Future Use
When reviewing Terraform wrappers, inspect pass-through variables against the receiving child module. Penalize mismatched attribute names, unused attributes, and parallel validation rules that claim behavior the child does not implement. Prefer removing the wrapper knob, matching the child type exactly, or adding an explicit transformation with focused tests.
