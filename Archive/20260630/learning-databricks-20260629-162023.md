---
type: short-term-learning
topic: databricks
created: 2026-06-29 16:20:23
source: codex-conversation
confidence: high
---

# Learning: external volume cloud URIs use volume privileges

## Context
While creating a Markdown/Mermaid diagram for Databricks Unity Catalog volume authentication and FUSE access, official Databricks docs were checked for the distinction between volume access and direct external-location access.

## Learning
For data under a Unity Catalog external volume, access through a cloud URI such as `s3://...` is governed by the external volume's privileges, not by `READ FILES` or `WRITE FILES` on the parent external location. Direct external-location privileges apply to direct path access outside a table or volume object. Normal volume reads require `USE CATALOG`, `USE SCHEMA`, and `READ VOLUME`; writes also require `WRITE VOLUME`.

## Future Use
When advising on UC volumes, FUSE `/Volumes/...`, or source-owned S3 access, keep `READ VOLUME`/`WRITE VOLUME` separate from `READ FILES`/`WRITE FILES`. Do not imply external-location grants are required to read an already-created external volume.
