---
type: short-term-learning
topic: databricks
created: 2026-06-11 18:46:52
source: codex-conversation
confidence: high
---

# Learning: Split Default And Explicit Compute Policy Approval Paths

## Context
During CAV-131446 compute policy planning, the user explained the desired operating model for Databricks team compute policies.

## Learning
When a team is assigned to a workspace, it should automatically receive tightly cost-limited default compute policies. Higher-cost, ML-oriented, or otherwise special cluster policies are a different approval process and should be modeled as explicit grants that can be approved at team creation time or later. The catalog should avoid treating explicit policies as overrides of default policies once the feature is split; explicit policies should be additive and use distinct profile/policy identities. The compute-policy Terraform module input should remain a normalized, lifecycle-agnostic primitive where possible. Simplification should primarily come from a cleaner workspace assignment output contract that exposes the canonical compute-policy principal directly.

## Future Use
For future compute policy work, prefer separate Terragrunt units or clearly separated configuration paths for automatic low-cost defaults versus approved explicit policies. Keep approval workflow semantics in the catalog layer and keep the module focused on creating policies and `CAN_USE` permissions from normalized instances.
