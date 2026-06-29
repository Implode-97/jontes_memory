---
type: short-term-learning
topic: databricks
created: 2026-06-26 12:04:30
source: codex-conversation
confidence: high
---

# Learning: Databricks file events SQS modes

## Context
While implementing source-owned external locations for the sandbox catalog platform, a review asked whether file events require a supplied SQS queue URL or can use Databricks' automatic queue path.

## Learning
For Databricks external locations, model file events as an explicit opt-in object instead of a standalone boolean. Use `file_events = null` for disabled, `mode = "managed_sqs"` for the Databricks-managed automatic path with no queue URL, and `mode = "provided_sqs"` only when a source-owned SQS queue URL is supplied. Do not expose a `managed_sqs_queue_url` input just because the provider schema has an optional field; the platform contract should stay simple unless a real caller needs that override.

## Future Use
When extending Databricks file-events IaC, keep validation, Terragrunt guards, tests, and source handoff aligned around these two modes. Treat "Automatic" in product/UI wording as the provider-backed `managed_sqs` mode.
