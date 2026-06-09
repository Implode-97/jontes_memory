---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:07:41
source: codex-conversation
confidence: high
---

# Learning: workspace wrapper main scoring

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/main.tf`, the user asked for a strict KISS/YAGNI Terraform wrapper review focused on readability, maintainability, pass-through logic, broad dependencies, and Databricks bootstrap safety.

## Learning
The wrapper main file is serviceable and readable but should score below the adjacent README because the security-group egress toggles, arbitrary additional pass-role ARNs, optional root bucket override, duplicate bucket-name guard logic, and broad workspace `depends_on` add cleanup-worthy surface area. The final `databricks_mws_permission_assignment` dependency on the full workspace module and group membership is justified because Databricks permission assignment can race identity federation/metastore assignment readiness.

## Future Use
For future reviews of this wrapper, preserve real AWS/Databricks bootstrap ordering while asking for narrower public knobs, clearer dependency comments or readiness outputs, and one obvious bucket naming path before freezing the module contract.
