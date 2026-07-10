---
type: short-term-learning
topic: databricks
created: 2026-07-06 13:45:14
source: codex-conversation
confidence: high
---

# Learning: External Location URL Normalization Can Cause Catalog Drift

## Context
While investigating `data-platform-terragrunt-catalog` CAV-133911 E2E failures, a timed no-op root apply succeeded at Terragrunt level but exposed non-idempotent sandbox catalog behavior from `data-platform-terraform-modules`.

## Learning
The Databricks provider can normalize bucket-root external location URLs from `s3://bucket` to `s3://bucket/` during refresh. If a wrapper feeds `module.external_location.url` into a downstream catalog `storage_root`, the next apply can plan `s3://bucket/managed` to `s3://bucket//managed` and force catalog replacement. The correct pattern is to use one canonical input-derived URL for both external-location input and catalog storage-root construction, and avoid exposing provider-normalized URL formatting as the wrapper's public contract.

## Future Use
When composing Databricks external locations with catalogs, do not derive desired resource inputs from provider-refreshed URL outputs. Add mock-provider coverage that overrides the external location URL with a trailing slash and asserts canonical wrapper outputs and storage roots remain stable.
