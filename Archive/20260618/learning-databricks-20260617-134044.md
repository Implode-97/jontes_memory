---
type: short-term-learning
topic: databricks
created: 2026-06-17 13:40:44
source: codex-conversation
confidence: high
---

# Learning: refactor_aula deployment fixes

## Context
During deployment of `/Users/jnyjc2/code/innovation-week/refactor_aula`, the Databricks CLI reached upload but failed through the Terraform bundle engine and then hit an invalid app user scope.

## Learning
For this project, keep `bundle.engine: direct` in `databricks.yml`. The Terraform-based DAB engine failed with an expired OpenPGP checksum-signature key while trying to download Terraform. The direct engine deployed successfully. Do not request `iam.current-user:read` in `user_api_scopes`; the workspace Apps create API rejected it as invalid, although existing app metadata may show it as an effective scope. Use only `sql` and `files.files` for this pass-through app unless a later concrete feature needs another accepted scope.

## Future Use
Deploy with profile `DEFAULT`, pass the real Firecrackers budget policy ID through `firecrackers_budget_policy_id`, then run the app resource. Verify with `databricks apps get` and app logs rather than relying only on `bundle summary`.
