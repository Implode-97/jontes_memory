---
type: short-term-learning
topic: terraform
created: 2026-05-18 17:49:02
source: codex-conversation
confidence: high
---

# Learning: prefer real provider confidence where practical

## Context
The user spent multiple Terraform sessions refining tests for S3, IAM role, workspace, and wrapper modules, including questions about `prevent_destroy`, mocks, and real apply behavior.

## Learning
The user's Terraform testing preference is to use real provider plan or apply tests where they give meaningful confidence and are practical to run. For ordinary resource modules, real apply tests are preferred when credentials and environment allow it. For expensive or slow resources such as full Databricks workspaces or broad wrappers, use real provider plan tests and mock-provider apply tests. Avoid brittle tests that rely on `prevent_destroy` as a cleanup safety net; if a resource is removed from code, Terraform no longer enforces that lifecycle rule.

## Future Use
When adding tests, focus on contract, naming, policy shape, invalid inputs, and lifecycle-sensitive behavior. State clearly when validation was limited to plan or mocks because full apply would be too slow or risky.
