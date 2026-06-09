---
type: short-term-learning
topic: terraform
created: 2026-06-08 23:30:11
source: codex-conversation
confidence: high
---

# Learning: Databricks metastore mock plan test is narrow and acceptable

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-metastore/tests/valid_plan_mock_provider.tftest.hcl`, the user asked for a Terraform plan-test semantics perspective focused on plan-known, stable, useful assertions for a one-resource module.

## Learning
For the Databricks account metastore child module, the mock plan test is high-scoring when it stays narrow: `mock_provider` with `override_during = plan`, one valid baseline variable set, and assertions that configured resource attributes match inputs plus `force_destroy` defaults to false. The `force_destroy` assertion is especially justified because the default is lifecycle-sensitive. The null `metastore_storage_root` output assertion is also useful when the module intentionally does not manage metastore-level storage, though it overlaps with apply coverage and depends on stable plan behavior for an unset storage root.

## Future Use
In future reviews, accept this file shape with only small cleanup suggestions. Do not ask for broader output preservation, computed ID checks, or extra scenarios in the mock plan file; keep computed ID behavior in mock apply tests.
