---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:14:16
source: codex-conversation
confidence: high
---

# Learning: External-location outputs should stay narrow

## Context
During the read-only multi-agent scoring sweep of `data-platform-terraform-modules/modules/databricks-workspace-external-location/outputs.tf`, reviewers compared the output API against the sandbox wrapper's actual consumers and current Databricks external-location behavior.

## Learning
For the Databricks workspace external-location module, `name`, `url`, `skip_validation`, `force_destroy`, and explicit `workspace_bindings` are defensible outputs because real wrappers consume them and they expose storage path, bootstrap validation, destroy-pending, and automatic-binding omission behavior. `workspace_ids` and `access_mode` can be useful consumer-facing summaries. The weaker outputs are provider facts or test-shaped mirrors such as `id`, `credential_id`, `isolation_mode`, `sse_algorithm`, and especially the duplicate `external_location` object that repeats scalar outputs.

## Future Use
When reviewing adjacent external-location, catalog, or storage-credential output APIs, preserve real wrapper/lifecycle/binding contracts and push provider internals into resource-focused tests unless a named downstream consumer needs them.
