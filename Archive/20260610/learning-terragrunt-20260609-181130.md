---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 18:11:30
source: codex-conversation
confidence: high
---

# Learning: dbcore workspace selectors need canonical generated paths

## Context
This came from reviewing `data-platform-terragrunt-live/non-prod/eu-central-1/dbcore/_workspace_provider.hcl` for new-maintainer readability, KISS, YAGNI, maintainability, and dependency-path safety.

## Learning
For live dbcore `_workspace_provider.hcl` selectors, `workspace_target_name` is valuable because CI target dependency inference keys from it, and it gives maintainers a clear workspace identity. The companion `workspace_dependency_path` should point at the actual generated workspace unit, such as the target stack's `.terragrunt-stack/dbx_workspace` directory, rather than an ambiguous or non-generated `dbx_workspace` shortcut. If target name, dependency path, and mock URL drift, CI can continue following the target name while Terragrunt reads a different or broken dependency path.

## Future Use
When reviewing similar selectors, reward explicit target identity but check the dependency path against the active `terragrunt.stack.hcl` unit path and generated stack layout. Recommend the smallest cleanup that makes the generated dependency path canonical.
