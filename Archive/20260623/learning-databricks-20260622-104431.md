---
type: short-term-learning
topic: databricks
created: 2026-06-22 10:44:31
source: codex-conversation
confidence: high
---

# Learning: External-location feature decisions

## Context
The user ran a subagent-backed planning loop for a Databricks Unity Catalog external-location feature in `sandbox_catalog`.

## Learning
The accepted V1 direction is a standalone regional `dbx_external_locations` feature. Source teams provide S3 URLs and IAM role ARNs from their AWS accounts; the platform creates UC storage credentials and external locations, owns them through the metastore owner group, and grants teams/data product service principals creator privileges. Default access is read-only and open to all metastore workspaces. Both `grp_team_<team_key>_contributor` and the data product SP application ID may have creator grants simultaneously; UC path-overlap errors and documented handoff are the conflict guardrail. Do not grant storage credential access to consumers.

## Future Use
Use `/Users/jnyjc2/code/vibe_code/sandbox_catalog/external-location-feature-plan.md` as the implementation plan. Preserve the two-step source IAM trust handoff, explicit storage credential keys, `READ_FILES` plus create external object grants, optional provided-SQS file events, and guarded lifecycle states.
