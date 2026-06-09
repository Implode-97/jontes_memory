---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:54:42
source: codex-conversation
confidence: high
---

# Learning: Keep Cross-Repo Team Fixtures Consistent

## Context
During the file-by-file scoring sweep, the `databricks-account-team-identities-wrapper` README and module tests used the example team key `weavebreakers`, while catalog examples, Terragrunt E2E tests, and remembered project context use `wavebreakers`.

## Learning
Fixture names in docs and tests are part of the readability contract when the same platform examples appear across Terraform modules, Terragrunt catalog examples, guard tests, and E2E flows. A near-spelling mismatch can make reviewers question whether two different teams are intended, even when the code itself may still be valid. This is especially important for Databricks team identity and sandbox examples, where team keys derive group names, cost centers, catalog names, and lifecycle outputs.

## Future Use
When reviewing or editing IaC docs/tests that use canonical example teams, check fixture names across module README files, module tests, Terragrunt examples, and E2E assertions. Prefer aligning to the established `firecrackers` and `wavebreakers` examples unless the user explicitly introduces a different team.
