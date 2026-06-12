---
type: short-term-learning
topic: terragrunt
created: 2026-06-11 18:30:29
source: codex-conversation
confidence: high
---

# Learning: Compute Policy Team Seeds Before Projection

## Context
The user asked to simplify the CAV-131446 `dbx_compute_policy` Terragrunt unit after discussing nested team groups and reducing list-heavy policy assembly.

## Learning
For team sandbox compute policies, default policies inferred from workspace assignments and explicit workspace-local requests should be normalized into minimal seeds first, then projected once into the module's `policy_instances` object shape. Use `merge(default_seed_map, requested_seed_map)` so explicit requests keep overriding inferred defaults for the same team/profile key. Keep product execution policy generation separate until V1 scope changes.

## Future Use
When simplifying Terragrunt units, look for duplicated resource-object assembly and collapse it into seed maps plus one final projection. Avoid carrying unnecessary fields in seeds; resolve details such as profile data and dependency principals in the final projection so validation errors remain useful and ordered.
