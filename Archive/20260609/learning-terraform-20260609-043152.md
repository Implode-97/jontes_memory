---
type: short-term-learning
topic: terraform
created: 2026-06-09 04:31:52
source: codex-conversation
confidence: high
---

# Learning: Lifecycle registries should not silently default empty

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/variables.tf`, reviewers agreed that the `teams` registry is the right public contract but questioned its `default = []`.

## Learning
For Terraform modules where a list input is a lifecycle-sensitive registry, an empty default can hide accidental omission and turn a real registry into a no-op. This is different from optional feature lists: here, missing rows can affect destruction, tombstone tracking, and Terragrunt row-removal guards. Prefer making such registry inputs required, or require callers to pass an explicit empty list when no managed rows are intended.

## Future Use
When reviewing Terraform/Terragrunt wrapper variables for lifecycle-managed registries, score required inputs higher than silent empty defaults unless an empty default is a deliberate, documented bootstrap path. Preserve validations for unique keys, lifecycle states, and downstream metadata.
