---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:10:03
source: codex-conversation
confidence: high
---

# Learning: AWS security group live test safety

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-security-group/tests/security_group_integration.tftest.hcl`, the user asked for an AWS/Databricks live-test safety perspective focused on whether the real-provider test is safe, valuable, and scoped for Databricks security group behavior.

## Learning
A real AWS `command = apply` security-group test can be valuable because it proves actual provider lifecycle behavior for separate security group rules, but it scores poorly when kept under default `tests/` with plain `terraform test`, a deterministic security group name, no owner/TTL tags, and hard-coded fixture region/VPC. The current Databricks contract also includes self TCP/UDP and outbound 443, 3306, 8443, 8444, 8445, reserved 8446-8451, plus conditional Lakebase 5432.

## Future Use
For future reviews, ask to move live apply tests to opt-in `real_tests/` or protected/manual CI, randomize names, add cleanup tags, make fixture region/VPC explicit, and keep default `tests/` mock-safe.
