---
type: short-term-learning
topic: terraform
created: 2026-06-09 06:25:30
source: codex-conversation
confidence: high
---

# Learning: Mock apply tests should not lock duplicate output shapes

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-catalog/tests/mock_apply.tftest.hcl`, reviewers compared the test's mock apply assertions against Terraform output API behavior and Databricks workspace-catalog binding semantics.

## Learning
For the workspace catalog module, a mock apply test is valuable when it protects the two real modes: selected workspaces create explicit read-write bindings for `workspace_ids - provider_workspace_id`, and an empty workspace set leaves the catalog open with no explicit bindings. The weaker pattern is asserting many input-derived scalar outputs plus a duplicate `catalog` object, because that preserves broad public API shape more than resource behavior.

## Future Use
When reviewing similar mock apply tests, keep direct resource assertions for lifecycle-sensitive behavior, defaults such as `force_destroy = false`, explicit binding count/keys, and binding type. Trim output mirroring to outputs with real downstream value, avoid asserting both scalar and composite duplicates, and only assert provider-schema internals such as `provider_config` when a known regression or contract depends on them.
