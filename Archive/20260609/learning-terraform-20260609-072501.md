---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:25:01
source: codex-conversation
confidence: high
---

# Learning: External-location variables need the binding invariant close to inputs

## Context
During a read-only readability/KISS/YAGNI scoring review of `data-platform-terraform-modules/modules/databricks-workspace-external-location/variables.tf`, the user asked whether a new platform maintainer could understand why each input exists and how to call the module safely.

## Learning
The variable file is compact and mostly shippable, with appropriate narrow validation for lowercase underscore Databricks object names, numeric workspace IDs, S3 URLs without spaces, and false defaults for destructive or validation-skipping booleans. The main cleanup is contract locality: `workspace_ids` and `provider_workspace_id` should briefly carry the Databricks automatic provider-workspace binding invariant, since `main.tf` leaves that binding unmanaged and replaces the external location when the provider workspace changes. Owner and comment should remain optional pass-through strings without trim or non-empty validation.

## Future Use
For future Unity Catalog external-location reviews, recommend narrow variable-description cleanup rather than rework. Preserve simple validation and avoid broadening the input surface; keep automatic-binding explanation near any selected-workspace/provider-workspace inputs.
