---
scope: domain
domain: databricks
topics: [compute, cluster-policy, sql-warehouse, tags, networking, spark, serverless-gpu]
stability: mixed
last-reviewed: 2026-07-10
---

# Databricks Compute

## Compute Policies And Cost Tags

- Treat policy defaults as reusable configurations, then generate or bind policies per team, role, service principal, or product.
- Required tags include owning team, billing cost center, principal name/type, data product, and deployment target.
- Product tags may extend but never overwrite platform cost attribution.
- Keep automatic low-cost defaults separate from approved larger, ML, or special policies.
- Default profiles arrive with workspace assignment; explicit profiles are additive and use distinct identities.
- Keep policy rendering separate from output shaping; broad rendered-policy or assignment outputs require observed consumers.
- In the nested team hierarchy, `CAN_USE` on the contributor group covers contributor, privileged contributor, and owner; avoid duplicate higher-role grants.
- Validate provider-compatible custom tag keys and values early, including spaces, `/`, unsupported characters, and reserved keys such as `.`, `..`, and `_index`.

## Spark I/O

- For I/O-bound volume workloads, start with file layout, scan partition sizing, file-open cost, input partition count, parallel discovery, and current runtime defaults.
- Spark file-source settings do not optimize custom task code that opens `/Volumes` paths; consider Delta/table layout or bounded per-task threaded I/O.
- `spark.task.cpus` is integral and cannot oversubscribe fractional task slots.

## AWS Networking

- Keep customer-managed VPC security-group rules separate for state clarity and use stable caller-selected keys for custom egress.
- Validate IPv4 CIDRs and avoid speculative future-port toggles.

## Serverless GPU / AI Runtime

- Serverless GPU is Databricks AI Runtime on serverless, not a classic GPU cluster or DBR ML replacement.
- Users select a serverless environment and accelerator from notebooks or jobs; libraries come from the environment or explicit dependencies.
- Govern rollout through preview controls, usage policies, tags, dependency standards, and documented notebook/job templates.

## Serverless Budget Policies

- Treat serverless budget policy as account-scoped, not automatically workspace-scoped.
- Empty workspace bindings are an explicit all-workspaces activation decision, not a harmless default.
- Name opt-in and blast radius clearly, and prevent later `extra_tags` merges from overriding platform owner or cost attribution.

## Current State — Verify

- Re-check current Databricks networking ports before freezing security-group rules; June 2026 guidance included self TCP/UDP, outbound 443, 3306, 8443–8445, reserved 8446–8451, and conditional Lakebase 5432.
- Re-check AI Runtime regions, preview status, accelerators, environment versions, Python, CUDA, Databricks Connect, and included ML libraries before making production recommendations.
