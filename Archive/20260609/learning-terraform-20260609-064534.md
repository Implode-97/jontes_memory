---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:45:34
source: codex-conversation
confidence: high
---

# Learning: Keep compute policy outputs narrow

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-compute-policy/outputs.tf`, reviewers compared the output API against Terraform output semantics, Databricks compute policy permissions, and current repo consumers.

## Learning
For Databricks compute policy modules, `policy_ids` and `policy_names` are strong outputs because they expose small provider-created facts keyed by the caller's logical instances. Broad composite outputs such as `policies_by_instance` score lower when they duplicate those maps and also publish caller metadata, ACL inputs, authoritative tag maps, and rendered policy JSON. `policy_assignments` is only justified if a downstream unit needs permission readiness or audit facts; otherwise it mostly freezes local flattening shape.

## Future Use
When scoring or editing Terraform outputs, treat child-module outputs as public contracts. Prefer narrow outputs with observed consumers, and keep rendered JSON or ACL assertions in resource-focused tests unless automation actually consumes that shape.
