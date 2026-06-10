---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:20:08
source: codex-conversation
confidence: high
---

# Learning: Thin Account Selectors Are An Intended Catalog Pattern

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/eu-central-1/ws_dev/_account.hcl` for readability and KISS/YAGNI. The repo README describes `example/non-prod-env/_targets.hcl` as a logical deployment target catalog used by per-folder `_account.hcl` selectors.

## Learning
In this catalog, a tiny `_account.hcl` file that explicitly selects one target such as `ws_dev` and re-exports the fields consumed by generic units is a justified locality pattern, not automatically overengineering. The maintainability risk starts when the selector exports duplicate shapes or safety-looking fields that current consumers do not use, such as descriptive account IDs that are not enforced.

## Future Use
For future Terragrunt catalog reviews, accept thin selector files when they keep unit templates generic and make folder-level target edits obvious. Prefer cleanup that removes or clearly frames unused exported metadata instead of replacing explicit selectors with path-derived magic.
