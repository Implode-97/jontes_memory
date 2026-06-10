---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 16:40:33
source: codex-conversation
confidence: high
---

# Learning: live Terragrunt target runner boundaries

## Context
This came from multi-agent scoring `data-platform-terragrunt-live/ci/scripts/run_stack_target.sh` for shell readability, KISS, YAGNI, Terragrunt stack behavior, and live CI safety.

## Learning
Live Terragrunt target runners score best when they stay thin around `terragrunt stack run` and make any extra preparation explicit. Current Terragrunt `stack run` already generates the selected stack before execution, so a separate target `stack generate` is readability debt unless it solves a documented stale-output case. Regional sibling generation can be justified when generated sibling units are real dependencies, but broad filesystem-based generation should be logged as a blast radius and preferably derived from declared target or dependency metadata.

## Future Use
When reviewing live Terragrunt CI helpers, preserve explicit command allow-lists, non-interactive execution, and useful CI banners. Penalize uncalled destructive command surfaces, unvalidated stack paths, broad sibling loops that can fail unrelated targets, and wrapper steps that duplicate current Terragrunt semantics without improving safety.
