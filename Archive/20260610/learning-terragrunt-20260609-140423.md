---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:04:23
source: codex-conversation
confidence: high
---

# Learning: Isolated Example Units Root Is Not Self-Explanatory

## Context
This came from scoring `data-platform-terragrunt-catalog/example/units/root.hcl` for readability, KISS, YAGNI, maintainability, structural simplicity, and safety-aware simplification.

## Learning
The `example/units` directory currently contains only a bare `root.hcl` that generates a simple AWS provider, while repo docs and active stack generation point maintainers to `example/non-prod-env/root.hcl` as the canonical example root. Without a current generated unit, README entry, or inline selection rule, this isolated root reads as an inactive placeholder. Its small size does not remove the maintainability issue because a new platform maintainer cannot tell when to use it instead of the documented example tree.

## Future Use
For future Terragrunt catalog reviews, score isolated example roots or provider parents down unless their active consumer and use case are obvious. Prefer removal before go-live, or add very tight documentation plus an actual current consumer if the separate root is intentionally supported.
