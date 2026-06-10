---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 17:57:18
source: codex-conversation
confidence: high
---

# Learning: live common vars as contract

## Context
This came from multi-agent scoring `data-platform-terragrunt-live/non-prod/_common_vars.hcl` for Terragrunt data shape, KISS, and live platform maintainability.

## Learning
A small `_common_vars.hcl` is a good live Terragrunt pattern when it centralizes real cross-stack facts such as naming prefix, remote-state bucket/region, Databricks account provider facts, and default tags. The file should be treated as a contract with catalog units and provider parents, not just a convenient constants bag. Missing locals consumed by active units, such as a deployer service-principal application ID, are higher-risk than ordinary tag cleanup because they break the shared contract.

## Future Use
When reviewing live common vars, preserve explicit stable enterprise facts and avoid over-abstraction. Check every active consumer contract, especially generated provider/unit locals. Penalize stale or test-looking ownership tags in live trees, and add only small comments for high-blast-radius backend/account values when edit safety is not otherwise obvious.
