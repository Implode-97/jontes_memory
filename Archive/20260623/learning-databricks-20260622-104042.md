---
type: short-term-learning
topic: databricks
created: 2026-06-22 10:40:42
source: codex-conversation
confidence: high
---

# Learning: External-location feature privilege contract

## Context
The user was finalizing a Databricks Unity Catalog external-location feature plan in `sandbox_catalog` and asked for a read-only UC/security review against current Databricks constraints.

## Learning
The accepted contract is a standalone `dbx_external_locations` feature with registry data in regional `dbcore`. Platform/metastore owners own the UC storage credentials and external locations; source teams provide explicit storage credential keys, external IAM role ARNs, and S3 URLs. Default external locations should be `read_only = true`, open to all workspaces unless the feature is explicitly changed, and should not grant teams/products storage-credential privileges. Creator principals may include both `grp_team_<team_key>_contributor` and a data product service principal at the same time. Final external-location creator grants should be explicit and narrow: `CREATE_EXTERNAL_TABLE`, `CREATE_EXTERNAL_VOLUME`, and `READ_FILES` by default; no `WRITE_FILES`, `MANAGE`, `ALL_PRIVILEGES`, `CREATE_MANAGED_STORAGE`, or `EXTERNAL_USE_LOCATION` by default.

## Future Use
When implementing or reviewing this feature, keep storage credential grants platform-only, model file events only as a complete supported object, and document UC path overlap errors plus handoff as the guardrail.
