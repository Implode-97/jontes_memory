---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:36:47
source: codex-conversation
confidence: high
---

# Learning: self-targeting workspace selectors score high

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/eu-central-1/ws_prd/_workspace_provider.hcl`, which contains `workspace_target_name = "ws_prd"` and `workspace_dependency_path = "../dbx_workspace"`.

## Learning
For `_workspace_provider.hcl` reviews, a selector under the matching workspace folder that points to that workspace's generated `dbx_workspace` unit is much safer and easier to maintain than a cross-workspace selector. The remaining readability concern is dependency-path base clarity: reviewers should check whether the path is interpreted relative to the generated stack unit, stack root, or region root, and compare it with local README/default conventions.

## Future Use
Score self-targeting selectors in the high range when folder name, target name, active stack unit, mock slug, and future workspace-scoped YAML align. Penalize only narrow cleanup issues such as ambiguous relative path conventions or missing explicit mock URL parity with live examples.
