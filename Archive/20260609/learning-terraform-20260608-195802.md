---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:58:02
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper variable API review

## Context
During the scoring sweep of `data-platform-terraform-modules`, the sandbox catalog wrapper `variables.tf` was reviewed for KISS, YAGNI, readability, and maintainability.

## Learning
The variable file is serviceable but too broad for a greenfield wrapper API. The justified complexity is the sandbox lifecycle contract, the guarded empty-list cleanup path, cost metadata requirements, and the `workspace_ids` / `provider_workspace_id` invariant caused by Databricks automatic creation-workspace bindings. The weaker parts are caller-supplied facts or knobs that can become duplicate sources of truth: separate AWS and Databricks naming prefixes when Terragrunt currently derives one from the other, `metastore_region` as a required input alongside `metastore_id`, permissive/fake-looking ARN and region validations, and public `skip_validation`.

## Future Use
For future sandbox-wrapper reviews, keep destructive-action and workspace-binding guards, but push for one source of truth for provider-owned facts and naming conventions. Treat broad validation-only API surface and test/emergency feature flags as cleanup candidates unless they protect a real failure mode.
