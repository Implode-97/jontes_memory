---
type: short-term-learning
topic: databricks
created: 2026-06-17 14:37:43
source: codex-conversation
confidence: high
---

# Learning: Data Product Catalog Grant Details

## Context
During implementation of Jira CAV-134270 in the sandbox catalog workspace, reviewer agents found Databricks-specific risks in the first data product catalog pass.

## Learning
For data product catalogs, grant the product service principal catalog privileges by its Databricks application ID, not display name. Keep break-glass grants as group display names, using `grp_team_<owner_team_key>_owner`. Data product catalog Terragrunt units that derive break-glass team groups should declare an ordering dependency on account team identities, as sandbox catalogs do, to avoid root stack apply races. Service principal OIDC federation policy names should be conservative lowercase hyphen-separated IDs, and wrapper validation should cap policies at 20 per data product environment.

## Future Use
When extending this feature, preserve application-ID grants for service principals, group-name grants for team break glass, team-identities ordering, and bootstrap-tolerant lifecycle guards. Avoid reintroducing display-name SP grants or underscore federation policy IDs.
