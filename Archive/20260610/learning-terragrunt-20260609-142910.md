---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:29:10
source: codex-conversation
confidence: high
---

# Learning: Keep Terragrunt guard tests real but table-driven

## Context
This came from scoring `data-platform-terragrunt-catalog/tests/terratest/guards/dbx_sandbox_catalogs/dbx_sandbox_catalogs_plan_test.go` for readability, KISS, YAGNI, maintainability, and guard-test value.

## Learning
Fast guard tests score well when they execute the same shell scripts and generated Terragrunt unit contracts that CI uses, while replacing live provider behavior with narrow fake `terraform output -json` fixtures. Generated-unit `hcl validate`, repo-copy fixtures, and fail-closed Terraform stubs are justified when they prove real hook/YAML/Terragrunt semantics without live AWS or Databricks cost. The readability debt appears when each lifecycle row repeats workspace/input/output/stub/env setup, helper return values are wider than callers use, or env merging becomes a mini framework.

## Future Use
When reviewing or writing Terragrunt guard tests, keep the real script/Terragrunt boundary, but make lifecycle matrices table-driven with small case runners. Narrow helper APIs to what callers use, centralize repeated Terragrunt env/options setup, and simplify env override handling unless deterministic de-duplication is clearly required.
