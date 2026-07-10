---
type: short-term-learning
topic: databricks
created: 2026-07-10 14:19:02
source: codex-conversation
confidence: high
---

# Learning: Copied federation policies make product environments one trust boundary

## Context
The CAV-134270 Terragrunt design copies each product-level OIDC federation policy unchanged to the separate service principal created for every environment cycle.

## Learning
Databricks workload identity token exchange selects the service principal with its application/client ID and then validates the IdP token against that principal's federation policy. If staging and production principals have identical issuer, audience, subject, and subject-claim rules, the same matching OIDC token can authenticate as either principal by selecting the corresponding client ID. Separate principals still improve permissions and audit identity, but they do not create separate authentication trust boundaries.

## Future Use
Ask explicitly whether all product environments should share repository-level trust. If production needs stronger isolation, make federation intent environment-specific and use distinct stable subjects, protected refs, or environment-aware subject composition supported by the IdP. If shared trust is intentional, document it so reviewers do not infer isolation from the separate service principals.
