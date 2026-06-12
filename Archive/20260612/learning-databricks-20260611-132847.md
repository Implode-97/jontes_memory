---
type: short-term-learning
topic: databricks
created: 2026-06-11 13:28:47
source: codex-conversation
confidence: high
---

# Learning: UC-First With Governed MLOps Data Planes

## Context
The user is shaping an enterprise Databricks-first data governance platform for thousands of users and petabyte-scale data, while partnering with MLOps teams that need GPU training in AWS regions where Databricks is unavailable or unsuitable.

## Learning
The durable platform stance should be "Unity Catalog is the source of truth for authorization, discovery, lineage, and model/data governance," not a literal "no physical S3 access ever" policy. Direct access to UC managed storage remains a hard no. For MLOps and autonomous-driving training, the platform must provide governed paved roads: UC tables and volumes for discovery and manifests, credential vending/Open APIs where supported, regional AWS training stacks such as SageMaker/HyperPod/EKS with tightly scoped short-lived or brokered S3 access, and write-back of lineage, artifacts, and models into UC/MLflow.

## Future Use
When advising on this platform, challenge blanket S3 bans that lack usable MLOps paths. Recommend productized exceptions with audit, TTL, owner, tags, and registry-driven IAM/S3/Lake Formation controls rather than ad hoc roles or direct bucket grants.
