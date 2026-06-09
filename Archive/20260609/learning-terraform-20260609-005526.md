---
type: short-term-learning
topic: terraform
created: 2026-06-09 00:55:26
source: codex-conversation
confidence: high
---

# Learning: Validate Resource-Owned Contracts First

## Context
During file-by-file scoring of the Databricks serverless budget policy module, the same issue appeared across variables, outputs, and tests: caller metadata was validated and echoed more strongly than provider/resource-owned behavior.

## Learning
For Terraform modules that accept caller-prepared objects, keep the module contract centered on fields the module actually uses to create resources or enforce safety. Avoid making taxonomy fields like `policy_type`, raw tags, or caller-owned names/principals into broad public output contracts unless a real downstream consumer needs them. Validate low-cost resource assumptions such as non-empty required principal lists when the module always emits a grant, but do not validate provider-owned existence or build large policy engines.

## Future Use
When reviewing or designing IaC modules, compare variables, outputs, and tests against `main.tf`. Prefer narrow outputs like provider-created IDs and direct resource assertions in tests. Treat broad input-echo outputs and tests that preserve them as pre-freeze cleanup candidates.
