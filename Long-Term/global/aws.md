---
scope: global
topics: [aws, profiles, account-safety, diagnostics]
stability: durable
last-reviewed: 2026-07-10
---

# AWS Preferences

## Account And Profile Safety

- Prefer a dev-scoped profile for Data Platform and dmp-dev diagnostics; verify `aws sts get-caller-identity` first.
- Read-only use of powerful local roles is acceptable only after confirming the caller is in dmp-dev account `498873881431`.
- Never fall back silently to a cross-account admin identity. State the selected profile, account, and role, and keep investigations read-only unless the user authorizes writes.

## Current State — Verify

- Local examples include `catalog-deploy` and `scania-bedrock`; use them only after caller-identity confirms the intended dmp-dev account.
