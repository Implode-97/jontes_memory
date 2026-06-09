---
type: short-term-learning
topic: databricks
created: 2026-06-09 07:34:04
source: codex-conversation
confidence: high
---

# Learning: Storage credential docs need external ID bootstrap accuracy

## Context
During read-only multi-agent scoring of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/README.md`, reviewers compared the README against current Databricks AWS storage credential documentation and the sandbox wrapper's Terraform flow.

## Learning
For AWS Databricks storage credentials, documentation should not imply the final IAM trust policy is fully composed before credential creation. The Databricks flow is placeholder trust, create the storage credential, retrieve the returned external ID, then update/finalize trust and self-assumption policy. In this platform, wrapper modules may create the storage credential with `skip_validation = true` because the final IAM trust depends on the returned external ID.

## Future Use
When reviewing or writing storage credential modules and wrappers, explicitly document caller-owned IAM bootstrap, returned external ID usage, and why validation may be skipped for the credential while later external-location validation remains meaningful.
