---
type: short-term-learning
topic: terragrunt
created: 2026-07-14 12:13:00
source: codex-conversation
confidence: high
---

# Learning: Sandbox file-events flag needs wrapper support

## Context
During a review of `data-platform-terragrunt-catalog`, a change tried to add `enable_file_events` to `units/dbx_sandbox_catalogs` inputs and assert `external_location_file_events_enabled` in the platform E2E stage.

## Learning
The currently available `aws-databricks-workspace-catalog-wrapper-sandbox` module sources, including fetched `origin/master` and feature branches in the local `data-platform-terraform-modules` checkout, do not declare `enable_file_events` in `sandbox_catalog_definitions` and do not output `external_location_file_events_enabled`. Terraform coerces extra object attributes away for typed object variables, so this Terragrunt field can be silently ignored rather than enabling file events.

## Future Use
Before accepting catalog-side wiring for sandbox file events, verify the published wrapper version actually exposes the input and output contract. If it does not, flag catalog changes that set or assert sandbox file events as CI/feature bugs.
