---
type: short-term-learning
topic: terragrunt
created: 2026-06-11 17:33:51
source: codex-conversation
confidence: high
---

# Learning: CAV-131446 catalog port vs modules output branch

## Context
While auditing the CAV-131446 compute policy forward-port, the user questioned whether Santosh's Terragrunt unit changes had been missed.

## Learning
The branch `feature/codex/CAV-131446-compute-policy-on-CAV-133921-platform-e2e` contains Santosh's catalog-side compute policy unit, example stack/provider edits, org policy YAML/JSON examples, guard test, and workspace assignment README changes. Those Terragrunt-facing files match `origin/feature/syahb2/CAV-131446-databricks-cluster-policy` byte-for-byte. The intentional divergence is in E2E coverage: Santosh's old monolithic `platform_e2e_test.go` plus `go.mod`/`go.sum` helper changes were replaced with modular E2E stage files that use the current helper contract. Separately, Santosh has a modules repo branch `origin/feature/syahb2/NO-JIRA-extend-output-contract` that adds `compute_policy_team_assignments`; it was not part of the catalog branch port.

## Future Use
When continuing CAV-131446, treat catalog Terragrunt changes as ported. Only add or depend on `compute_policy_team_assignments` after deciding to include the separate modules output-contract branch or after that module change is merged/released.
