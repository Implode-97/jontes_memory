---
type: short-term-learning
topic: databricks
created: 2026-06-22 10:41:21
source: codex-conversation
confidence: high
---

# Learning: dbx_external_locations planning defaults

## Context
The user requested a KISS/YAGNI and enterprise-sustainability review for a planned standalone `dbx_external_locations` feature in `sandbox_catalog`.

## Learning
Accepted direction: keep the feature standalone; source teams provide external AWS IAM role ARNs and S3 URLs; the platform owns Unity Catalog storage credentials, external locations, and grants; default to read-only and open workspace scope; allow both a team contributor group and a data product service principal to receive creator privileges at the same time; use explicit storage credential keys; only add a nested `file_events` object when the pinned Databricks provider supports the full queue configuration. The critical risk is the source-owned IAM trust handoff: storage credential external IDs must be surfaced and source teams must update role trust/policies before external-location validation can be reliable.

## Future Use
For implementation plans, keep Terraform modules principal- and team-agnostic, let Terragrunt resolve team/product principals, avoid permanent `skip_validation`, and document a two-step or stateful trust bootstrap path.
