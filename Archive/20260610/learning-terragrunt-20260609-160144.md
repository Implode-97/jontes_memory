---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 16:01:44
source: codex-conversation
confidence: high
---

# Learning: workspace assignment unit activation and guard ownership

## Context
This came from multi-agent scoring of `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/terragrunt.hcl` for readability, KISS, YAGNI, maintainability, and Databricks workspace-assignment safety.

## Learning
The unit's account/workspace provider split, workspace dependency, team-identity ordering, generated lifecycle output, guard input, and pre-plan/pre-apply lifecycle hooks are justified because they protect workspace placement state that Terraform cannot infer after YAML row removal. The readability debt is not the safety model itself, but inactive or split ownership: the active example stack does not declare the unit, root CI excludes its dedicated guard package, registry consistency is partly enforced by an unused fail-fast local and partly by a hook, and active team filtering is repeated across locals, generated JSON, hooks, and final inputs.

## Future Use
For similar Terragrunt units, preserve real destructive lifecycle guards but score down safety-looking dead locals, inactive stack/CI wiring, opaque packaging hooks, fallback dependency paths, and generated guard files whose names or hook commands hide their actual ownership.
