---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:29:33
source: codex-conversation
confidence: high
---

# Learning: Hybrid real/mocked smoke tests need live-only value

## Context
During read-only scoring of `databricks-workspace-assignments-wrapper/tests/real_plan_smoke.tftest.hcl`, the user asked for multi-agent KISS/YAGNI review without local test execution.

## Learning
A Terraform test that mixes a real account provider with a mocked workspace provider can be valid, but it scores poorly when the fixture uses a fake workspace ID and overrides workspace lookups. That shape is neither a true live workspace smoke nor a fully mock-safe unit test. If it also lives under default `tests/` discovery, the live credential/account cost must be justified by assertions that prove facts mocks cannot.

## Future Use
When reviewing Terraform smoke tests, distinguish real-provider posture from real signal. For hybrid tests, ask what remains live after mocks and overrides. Recommend an opt-in live lane or pure mock test unless the file proves provider auth, live lookup behavior, API constraints, or provider-computed facts that normal mock plan/apply coverage cannot.
