---
type: short-term-learning
topic: terragrunt
created: 2026-05-18 17:49:04
source: codex-conversation
confidence: high
---

# Learning: Terragrunt E2E tests must clean up reliably

## Context
Later catalog-repo sessions around team identities and sandbox catalogs repeatedly hit failed pipeline runs that left Databricks groups behind after apply or destroy errors.

## Learning
For shared Databricks Terragrunt E2E tests, cleanup reliability is a first-order design concern. The root stack test should own setup, apply, assertions, destroy, and cleanup, and guard or pre-hook tests should catch malformed generated inputs before real apply creates resources. Terraform output assertions are useful, but higher confidence comes from Databricks API checks that resources really exist and were removed. Tests should not depend on one fragile deferred destroy path when a failure can leave account-level objects behind.

## Future Use
When designing E2E tests, add fast guard tests before real deploys, keep destroy behavior observable, and consider fallback cleanup for resources that are easy to strand. Prioritize readability, but do not accept tests that routinely require manual cleanup.
