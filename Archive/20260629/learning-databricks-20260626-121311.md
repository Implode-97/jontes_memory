---
type: short-term-learning
topic: databricks
created: 2026-06-26 12:13:11
source: codex-conversation
confidence: high
---

# Learning: Source external locations avoid READ_FILES

## Context
During review of the sandbox catalog `dbx_external_locations` V1 implementation, the user rejected granting `READ_FILES` on source-owned external locations because it can bypass the intended Unity Catalog volume activation path.

## Learning
For source-owned external locations in this platform, creator grants should not include `READ_FILES`. The V1 grant set should stay focused on enabling governed object creation through Unity Catalog: `CREATE_EXTERNAL_TABLE` and `CREATE_EXTERNAL_VOLUME` on the external location. This supersedes earlier memory that treated `READ_FILES` as part of the narrow creator grant set for this feature.

## Future Use
When designing or reviewing external-location grants for source-owned data, avoid broad direct file-read privileges by default. Keep Terragrunt assignment resolution separate from Terraform grant creation, but make Terraform receive simple per-location resolved creator principal lists where practical.
