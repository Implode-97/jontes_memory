---
type: short-term-learning
topic: databricks
created: 2026-06-08 20:08:35
source: codex-conversation
confidence: high
---

# Learning: Name hidden Databricks readiness dependencies

## Context
While scoring the AWS Databricks workspace wrapper implementation, reviewers distinguished normal Terraform expression dependencies from Databricks hidden readiness dependencies, especially MWS permission assignment after metastore assignment enables identity federation.

## Learning
For Databricks workspace bootstrap modules, explicit `depends_on` can be justified when the provider/API has a hidden readiness condition, such as identity federation being available only after metastore assignment. Broad module-level `depends_on` still needs a precise comment or a narrower readiness output, because Terraform otherwise prefers expression references and broad dependencies can make plans more conservative.

## Future Use
When reviewing Databricks Terraform wrappers, keep explicit dependencies that protect documented provider/API readiness races, but require comments that name the exact hidden readiness. Challenge broad module-level dependencies and public wrapper knobs unless they protect a current concrete platform requirement.
