---
type: short-term-learning
topic: terragrunt
created: 2026-07-13 17:40:29
source: codex-conversation
confidence: high
---

# Learning: Keep dependency outputs out of Terragrunt locals

## Context
While integrating standalone Databricks CI identities, generated-unit tests exposed Terragrunt evaluation-order and mock-output pitfalls.

## Learning
Terragrunt `locals` should normalize only file/config data; dependency outputs must be resolved in dependency-aware blocks such as `inputs` or `generate.contents`. Referencing `dependency.*` from locals can make the entire configuration appear to have undefined dependencies during render. Mocks also need to remain tenant-generic: derive mock keys from the same YAML registries as real references, and generate a deterministic, distinct UUID-shaped application ID per logical service-principal key. Reusing one mock UUID collapses distinct principals in Terraform sets and can falsely reject valid plans. Downstream guards should require an exact enabled lifecycle state and UUID application ID rather than merely checking key existence.

## Future Use
For new Terragrunt integrations, separate dependency-independent request specs from dependency-resolved module inputs, use dynamic deterministic mocks, and add multi-principal render tests plus malformed-output fail-closed tests.
