---
type: short-term-learning
topic: obsidian
created: 2026-05-18 17:37:41
source: codex-conversation
confidence: high
---

# Learning: memory vault is backed by GitHub

## Context
The Obsidian Codex memory root was initialized as a Git repository and pushed to `Implode-97/jontes_memory` on GitHub.

## Learning
The memory vault now tracks its contents in Git on branch `main`, with `origin` set to `https://github.com/Implode-97/jontes_memory.git`. Repo-local copies of `recall-memory`, `capture-memory`, and `dream-memory` live under `skills/` so they can be reused elsewhere. The active `dream-memory` skill was updated to create a dated local commit after successful dream consolidation.

## Future Use
When future sessions modify memory, check Git status in the memory root. Dream runs should commit their completed consolidation changes with a short relevant dated message, but should not push unless explicitly asked.
