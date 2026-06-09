---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:01:24
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper version contract score

## Context
During the sandbox catalog wrapper scoring sequence, `aws-databricks-workspace-catalog-wrapper-sandbox/version.tf` was reviewed for readability, KISS, YAGNI, maintainability, and provider-contract clarity.

## Learning
The provider contract is worth keeping as-is, scoring about 89/100. The file is short, locally consistent, and each provider requirement maps to real wrapper behavior: AWS for caller identity and IAM policy documents, Databricks with `databricks.account` for account-level Unity Catalog assume-role policy generation while the default provider remains workspace-scoped, and `time` for the IAM propagation wait. The only minor readability gap is that the version floors do not document which provider feature or repo standard forced each minimum.

## Future Use
For later reviews of this module, treat the provider requirements as one of the cleaner parts of the wrapper. Do not suggest removing `databricks.account` or `time` unless the corresponding account-level data source or propagation wait is removed. Consider version-floor comments only if the repo starts documenting provider feature minimums consistently.
