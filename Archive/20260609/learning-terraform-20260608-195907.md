---
type: short-term-learning
topic: terraform
created: 2026-06-08 19:59:07
source: codex-conversation
confidence: high
---

# Learning: Match safety variable docs to actual wiring

## Context
While scoring the Databricks sandbox catalog wrapper `variables.tf`, reviewers found that `skip_validation` was documented as covering both storage credentials and external locations, but the wrapper hard-codes storage credentials to skip validation during bootstrap and only passes the variable to external locations.

## Learning
For Terraform module APIs, safety and validation flags must describe exactly which resources they affect. A broad description makes the contract look safer or more general than the implementation, especially when one resource has a deliberate hard-coded bootstrap behavior and another remains caller-controlled.

## Future Use
When reviewing or implementing Terraform variables, cross-check variable descriptions against every downstream use site. Prefer precise wording like "external location validation" over broad wording like "storage credentials and external locations" unless both are actually wired to the variable.
