---
type: short-term-learning
topic: databricks
created: 2026-06-29 17:51:00
source: codex-conversation
confidence: high
---

# Learning: validate Databricks SQL warehouse custom tags early

## Context
While reviewing Neenu's `data-platform-terraform-modules` MR for a workspace-scoped serverless SQL warehouse module, the implementation required cost tags and rendered them as `databricks_sql_endpoint.tags.custom_tags`.

## Learning
For Databricks SQL warehouse modules, required cost-tag presence is not enough. Databricks documents AWS-backed custom tag restrictions for keys and values: no spaces or `/`, only the supported AWS tag character set, and reserved keys such as `.`, `..`, or `_index` are invalid. If module validation only checks non-empty tags, bad team or workspace metadata can pass Terraform validation and fail later at provider/API apply time.

## Future Use
When reviewing or implementing SQL warehouse, serverless budget, cluster policy, or other Databricks compute-tagging modules, check both required platform attribution tags and provider-compatible tag key/value validation. Prefer compact invalid-input mock tests for representative invalid tag characters and reserved keys.
