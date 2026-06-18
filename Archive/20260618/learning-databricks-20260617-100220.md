---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:02:20
source: codex-conversation
confidence: high
---

# Learning: Refactor Aula Pass-Through Plan

## Context
The user asked to prepare `/Users/jnyjc2/code/innovation-week/refactor_aula` for a future refactor modeled on the `everything_app` Databricks Apps pass-through PoC. Three xhigh subagents reviewed auth, DAB/deployment, and architecture/UI risks.

## Learning
The refactor handoff now lives in `refactor_aula/REFACTOR_PLAN.md`, with project-local Databricks skills copied under `refactor_aula/.agents/skills` and project guidance in `refactor_aula/AGENTS.md`. A compact `everything_app` knowledge base was written to `refactor_aula/EVERYTHING_APP_EXPERIENCE.md`. The main required boundary is user pass-through only: remove app-SP SQL/UC/volume resource grants, stop using default `WorkspaceClient()` for data access, remove token-bearing `st.cache_resource`, and eliminate Files API fallback to app-SP.

## Future Use
Before implementing in `refactor_aula`, read those three files. Treat `REFACTOR_PLAN.md` as the execution checklist and preserve the no-app-SP-data-permissions goal unless the user explicitly changes the security model.
