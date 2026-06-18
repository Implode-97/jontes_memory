---
type: short-term-learning
topic: databricks
created: 2026-06-17 15:08:13
source: codex-conversation
confidence: high
---

# Learning: Data Product Output Contract Narrowed

## Context
During CAV-134270 implementation review, the data product catalog wrapper output API was considered too broad because it exposed duplicate top-level grant, OIDC, and empty workspace-binding maps.

## Learning
For V1 data product catalogs, keep `data_products` as the single per-product environment output contract. Each row should nest `catalog`, `service_principal`, `grants`, and `oidc_federation_policies`, while lifecycle and shared regional outputs remain separate. Do not expose empty workspace-binding outputs for this feature, because workspace assignment is owned by a future sibling feature.

## Future Use
When extending or testing data product catalogs, assert against the nested `data_products` contract instead of mirroring low-level module internals. Add new outputs only when another unit genuinely needs them.
