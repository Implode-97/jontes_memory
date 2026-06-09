---
type: short-term-learning
topic: databricks
created: 2026-06-08 23:24:04
source: codex-conversation
confidence: high
---

# Learning: Keep metastore outputs narrow and explicit

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-metastore/outputs.tf`, the user asked for a pragmatic platform maintainer judgement on downstream usefulness, naming, descriptions, and whether `try(storage_root, null)` is clear or too clever.

## Learning
For the Databricks account metastore child module, explicit outputs for metastore ID, name, region, and owner are useful downstream contract fields for wrappers, assignments, and documentation. They should be treated as narrow and acceptable, not broad child passthrough. The storage root output is the only weak point because the module intentionally omits metastore-level storage; exposing it can imply a supported managed-storage contract unless the description and value make the null-by-design behavior obvious.

## Future Use
In future reviews, accept this output shape with narrow cleanup. Prefer direct `null` for `metastore_storage_root` if the module deliberately never sets storage root, or direct attribute access if it may become a real output later; avoid `try(...)` unless guarding a genuine dynamic optional shape.
