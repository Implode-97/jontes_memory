---
type: short-term-learning
topic: databricks
created: 2026-06-09 04:41:17
source: codex-conversation
confidence: high
---

# Learning: Databricks system group entitlements are being locked

## Context
During a read-only scoring review of `databricks-workspace-assignments-wrapper/README.md`, current Databricks entitlement docs were checked against a Terraform workflow that hardens the built-in workspace `users` group.

## Learning
Databricks documents a staged entitlement behavior change for workspace system groups. Starting June 15, 2026, workspaces can opt in to explicit entitlement selection when principals are added; auto-enable starts July 27, 2026; enforcement for all workspaces starts September 14, 2026. Under the new behavior, `users` has no entitlements, `admins` has all workspace entitlements, and attempts to modify system group entitlements through Terraform, SCIM APIs, or scripts fail.

## Future Use
When reviewing Databricks workspace assignment or entitlement modules, flag README or implementation paths that present Terraform-managed `users` hardening as a durable workflow. Prefer docs that distinguish legacy behavior from the new locked-system-group behavior and target entitlement management at account groups where possible.
