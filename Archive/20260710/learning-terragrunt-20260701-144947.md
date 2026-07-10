---
type: short-term-learning
topic: terragrunt
created: 2026-07-01 14:49:47
source: codex-conversation
confidence: high
---

# Learning: External location E2E lifecycle applies

## Context
While investigating `data-platform-terragrunt-catalog-hotfix` branch `feature/codex/external-locations-v1`, the external-location E2E stage was found to have moved from always running a lifecycle matrix to gating it behind `RUN_EXTERNAL_LOCATIONS_LIFECYCLE_E2E`.

## Learning
The normal shared platform E2E contract for this catalog is one root `terragrunt stack run apply`, feature output assertions, and deferred cleanup. The external-location lifecycle matrix adds one intentionally failing apply plus four successful root applies after mutating `example/non-prod-env-e2e/eu-central-1/dbcore/external_locations.yml`. That is not Go/Terratest parallelism; it increases live Databricks/Terragrunt convergence surface and can rerun provider init, dependency output resolution, hooks, and Unity Catalog operations across the whole generated stack.

## Future Use
For `dbx_external_locations`, keep lifecycle transition and unsafe-delete behavior in fast guard tests by default. Treat live lifecycle E2E as an explicit opt-in stability/cost probe, not as the normal CI path, unless the team deliberately accepts repeated full-stack applies.
