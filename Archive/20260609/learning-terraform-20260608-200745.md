---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:07:45
source: codex-conversation
confidence: high
---

# Learning: Workspace wrapper dependency graph review

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/main.tf`, the focus was Terraform dependency graph readability, provider aliasing, `check`/postcondition usage, and explicit `depends_on`.

## Learning
The wrapper is mostly clean and shippable from a graph/provider-semantics perspective. AWS and Databricks account facts are derived from provider data sources, the `databricks.mws` alias is used for account APIs, subnet postconditions and derived bucket-name checks are justified validation, and the `mws_permission_assignment` dependency on the full workspace module is justified by the known metastore/identity-federation race. The broad workspace module `depends_on` is conservative but defensible because child outputs expose primary IDs/names, not full IAM policy, bucket policy/security settings, or security group rule readiness.

## Future Use
For later reviews, do not suggest removing the permission-assignment dependency. Prefer narrow cleanup: explain the hidden readiness dependencies better, especially group membership ordering, and only replace broad module-level `depends_on` if child modules expose explicit readiness outputs.
