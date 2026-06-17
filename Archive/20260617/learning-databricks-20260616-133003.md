---
type: short-term-learning
topic: databricks
created: 2026-06-16 13:30:03
source: codex-conversation
confidence: medium
---

# Learning: Omnigent Security Posture

## Context
The user asked whether Databricks-announced Omnigent/“Omniagent” collects or stores data and whether it is risky for Codex-style use.

## Learning
As of 2026-06-16, Omnigent should be described as an open-source, self-hosted or local agent harness rather than a Databricks SaaS that automatically collects repositories. The main risk is configured persistence and forwarding: session messages, tool calls, tool outputs, usage, files/artifacts, and comments can be stored in the Omnigent database/artifact store; prompts and context go to the selected model provider; optional OpenTelemetry/MLflow tracing can export metadata, and content capture is explicitly risky because messages may contain PII or secrets. Bare local server auth is off by default, while deployed/containerized servers enable auth by default.

## Future Use
For future recommendations, advise local-only trials, no sensitive repos first, disabled update check/tracing unless needed, no `OMNIGENT_OTEL_CAPTURE_CONTENT`, strict sandbox/env passthrough, authenticated OIDC/accounts for shared deployments, and careful review of provider/model retention policies.
