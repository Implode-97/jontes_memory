---
type: short-term-learning
topic: terragrunt
created: 2026-06-03 11:21:48
source: codex-conversation
confidence: high
---

# Learning: sandbox E2E propagation wait is diagnostic

## Context
While stabilizing CAV-132831 platform E2E, the user chose to add a naive wait before the sandbox catalog unit applies, to test whether fresh Databricks workspace/metastore propagation is causing external-location READ failures.

## Learning
`dbx_sandbox_catalogs` now has an apply-only `before_hook` with a default 900-second wait, controlled by `DBX_SANDBOX_PREREQ_WAIT_SECONDS`. This is intentionally a diagnostic/stabilization step, not a final readiness design. The sandbox module itself has a passing apply test when upstream resources are fixed, so this wait is testing the Terragrunt E2E boundary after fresh workspace/metastore creation.

## Future Use
If E2E passes with the wait, treat it as evidence of Databricks propagation timing and consider replacing the fixed sleep with a readiness check or narrowing the wait. Do not expand sandbox lifecycle coverage until the baseline path is stable.
