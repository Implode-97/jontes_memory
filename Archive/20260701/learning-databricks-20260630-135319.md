---
type: short-term-learning
topic: databricks
created: 2026-06-30 13:53:19
source: codex-conversation
confidence: high
---

# Learning: Keep Data Product Catalogs Out Of Compute Policy Intent

## Context
During CAV-134270 data product catalog cleanup, the user challenged why `compute_profile_ids` was normalized and exposed by `dbx_data_product_catalogs` even though the data product wrapper did not consume it for any resource.

## Learning
For this platform, data product catalog features should not carry compute-profile or cluster-policy intent unless a current implemented resource needs it. The catalog feature owns service principals, OIDC federation, buckets, external locations, catalogs, lifecycle, and catalog grants. Compute execution access belongs in sibling compute policy or workspace assignment features, with any future bridge designed deliberately rather than leaked through unused catalog metadata.

## Future Use
When editing data product catalog modules or Terragrunt units, avoid adding unused cross-feature fields such as `compute_profile_ids`. Keep product execution policy inference or grants in the compute/workspace feature boundary, based on explicit requirements and implemented consumers.
