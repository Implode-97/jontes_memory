---
type: short-term-learning
topic: architecture
created: 2026-07-13 10:27:13
source: codex-conversation
confidence: medium
---

# Learning: Tenant-scoped S3 event delivery is under design

## Context
The user is building a multi-tenant PaaS and is deciding whether S3-to-SQS event delivery should be enabled for every tenant by default or offered as an opt-in capability.

## Learning
The platform design needs to distinguish provisioning an idle SQS queue from attaching an active S3 event-notification rule. The user is cost-conscious and wants unused tenant capabilities to avoid background usage charges. No final architecture decision has yet been confirmed; opt-in event delivery is currently the likely direction.

## Future Use
When discussing this PaaS, evaluate eventing choices across cost, tenant isolation, shared-bucket notification quotas, retention, delivery semantics, and operational overhead. Do not assume a per-tenant bucket or per-tenant queue; verify the storage tenancy layout before proposing implementation details. Recheck current AWS pricing rather than relying on this note.
