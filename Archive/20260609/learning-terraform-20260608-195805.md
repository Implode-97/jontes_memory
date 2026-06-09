---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:58:05
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper variable contract score

## Context
During the sandbox catalog wrapper scoring series, `aws-databricks-workspace-catalog-wrapper-sandbox/variables.tf` was reviewed for readability, KISS, YAGNI, and maintainability from a new platform maintainer perspective.

## Learning
The variable contract is shippable but needs narrow cleanup, scoring about 78/100. Its naming inputs, row validation, empty-list cleanup guard, provider workspace invariant, and tombstone metadata exceptions are mostly justified by real Unity Catalog and cleanup failure modes. The main readability gaps are that `provider_workspace_id` does not explain Databricks automatic provider-workspace binding, `sandbox_catalog_definitions` relies on validations and README context to explain the tombstone row model, `skip_validation` is described broader than its actual external-location usage, and the optional permissions-boundary ARN validation uses fake partition flexibility.

## Future Use
For future edits or reviews, preserve lifecycle and workspace-binding safety guards. Prefer small contract-documentation cleanup: group variables by deployment facts, workspace binding, sandbox rows, and safety switches; clarify provider-workspace anchoring; describe destroyed tombstones inline; fix `skip_validation` wording; and hard-code commercial AWS ARN validation where appropriate.
