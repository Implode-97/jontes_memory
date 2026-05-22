---
type: short-term-learning
topic: databricks
created: 2026-05-21 11:19:48
source: codex-conversation
confidence: high
---

# Learning: Real Databricks tests must use the real deployer SP ID

## Context
While fixing the Databricks deployer admin-access module MR, the real-provider Terraform tests had placeholder UUIDs for `deployer_service_principal_application_id`, causing service-principal lookups to fail in CI.

## Learning
For the shared Databricks account-level module test environment, use `f425e414-e206-48d5-80ab-6d537930d0d7` as the real deployer service-principal application/client ID in real-provider plan and apply tests. This is a real test-environment identifier, not a mock placeholder.

## Future Use
When editing real-provider Databricks tests in this repository, keep pinned IDs tied to actual shared test resources. Do not use all-zero or made-up UUIDs in files named `real_*` or tests that contact live providers.
