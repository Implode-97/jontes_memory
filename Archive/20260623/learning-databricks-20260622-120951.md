---
type: short-term-learning
topic: databricks
created: 2026-06-22 12:09:51
source: codex-conversation
confidence: high
---

# Learning: External-location validation disabled

## Context
This updates the `dbx_external_locations` feature plan after the user clarified ownership boundaries for source-team S3 buckets and IAM roles.

## Learning
For the standalone external-location feature, Databricks validation should be disabled for both UC storage credentials and external locations. The platform does not manage the source S3 bucket, IAM role, bucket policy, or optional event queue, so Terraform applies should not fail because those source-owned pieces are misconfigured. Source teams are responsible for resolving runtime S3/IAM/queue access failures.

## Future Use
In future implementation sessions, default and enforce `skip_validation = true` for managed storage credentials and external locations in V1. Reject `skip_validation = false` in the registry/guard path unless the user deliberately changes the feature contract.
