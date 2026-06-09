---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:50:43
source: codex-conversation
confidence: high
---

# Learning: mock apply tests should not freeze broad output APIs

## Context
During read-only scoring of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/tests/mock_apply.tftest.hcl`, reviewers compared strong resource-level assertions with weaker output mirror assertions.

## Learning
Terraform mock apply tests are valuable when they assert provider-safe behavior directly on resources, such as Databricks open versus isolated mode, AWS role wiring, validation bootstrap flags, and explicit workspace-binding resources. The weak pattern is using mock apply tests to preserve broad scalar and composite output mirrors that are already candidates for API pruning. Assertions over `output.storage_credential.*`, duplicated scalar fields, or provider internals like `provider_config` can make cleanup harder without improving confidence.

## Future Use
When reviewing Terraform mock apply tests, keep assertions that protect behavior and lifecycle invariants. Push output assertions toward stable downstream contracts only, and avoid letting tests anchor broad output surfaces that should be narrowed before module APIs freeze.
