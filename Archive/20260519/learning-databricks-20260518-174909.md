---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:09
source: codex-conversation
confidence: high
---

# Learning: cluster policies are per principal for cost tracking

## Context
The cluster policy design thread used the grill-me workflow to clarify how compute limits and cost attribution should work for teams, products, service principals, and jobs.

## Learning
Cluster policy defaults should be reusable configurations, not one shared policy granted to everyone. Generate or bind policies per team, role, service principal, or product principal so mandatory tags remain accurate. Mandatory tags should include owning team, billing cost center, principal name, principal type, data product, and deployment target. Product teams may add their own finer-grained tags as long as they cannot overwrite platform cost tracking. Default policy profiles should be platform-owned, with additional larger policies declared separately and intentionally bound.

## Future Use
When implementing cluster policy IaC, protect deterministic generated names and mandatory tags. Avoid shared defaults that erase cost ownership. Keep optional free-text job tags out of platform-managed core unless the platform can validate them safely.
