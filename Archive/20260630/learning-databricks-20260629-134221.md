---
type: short-term-learning
topic: databricks
created: 2026-06-29 13:42:21
source: codex-conversation
confidence: high
---

# Learning: constrain Databricks cloud credentials at the final enforcement point

## Context
The user asked whether a Databricks service credential or instance profile could assume an intermediate AWS IAM role with explicit denies, then assume a team-owned role, to stop end users from writing to external storage while still enabling some trust.

## Learning
For Databricks governance, do not treat an intermediate IAM role with negative permissions as a durable guardrail once user code can assume another role. AWS role chaining does not make source-role S3 denies carry into the target role session. The enforceable controls are on the final role session, the target role policy or permissions boundary, S3 bucket/resource policies, AWS Organizations SCP/RCP guardrails, or Databricks Unity Catalog primitives that avoid exposing raw broad cloud credentials.

## Future Use
When advising on service credentials, instance profiles, or external storage onboarding, prefer platform-owned UC storage credentials/external locations, narrow grants, restricted instance profile usage, and validation of source-side IAM/bucket policies. Flag broad `sts:AssumeRole` from Databricks credentials as a governance escape hatch.
