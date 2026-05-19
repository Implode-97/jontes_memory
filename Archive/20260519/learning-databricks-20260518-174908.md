---
type: short-term-learning
topic: databricks
created: 2026-05-18 17:49:08
source: codex-conversation
confidence: high
---

# Learning: data products are service-principal managed

## Context
Planning sessions for data product catalogs, cluster policies, and GitLab federation repeatedly described the intended lifecycle for productionized data products.

## Learning
Productionized Databricks data products should be managed by a dedicated service principal tied to the product's GitLab repository, preferably through OIDC or federation rather than long-lived raw tokens. Team groups should not directly control the product. Development happens in the team's sandbox; staging or UAT validates changes before production; jobs normally run in the prod workspace for prod targets. Catalog naming examples use `dp_<product>_<target>`, such as `dp_car_images_prd` or `dp_car_images_uat`, while keeping flexibility for teams that want SIT/UAT variants.

## Future Use
When designing data product catalog IaC, separate team collaboration from product deployment identity. Model repo-bound service principals, federation policy, catalog ownership, and target environments explicitly instead of letting team group permissions blur the lifecycle.
