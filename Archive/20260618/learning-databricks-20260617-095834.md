---
type: short-term-learning
topic: databricks
created: 2026-06-17 09:58:34
source: codex-conversation
confidence: high
---

# Learning: refactor_aula should copy everything_app pass-through structure

## Context
During a read-only review of `/Users/jnyjc2/code/innovation-week/refactor_aula`, the user asked to compare architecture, Streamlit UI, testability, and maintainability against `/Users/jnyjc2/code/innovation-week/everything_app`.

## Learning
`refactor_aula` is a Databricks Streamlit labeling app that currently mixes UI, auth, config, data access, annotation writes, image loading, and notebook utilities. Future refactors should copy `everything_app` patterns: thin `app.py`, explicit config/auth modules, case-insensitive forwarded user-token handling, pure SQL helper functions with identifier/literal validation, no `st.cache_resource` for user-token-bound clients or data, and focused unit tests for helpers.

## Future Use
When working on `refactor_aula`, keep the app pass-through-first unless the goal explicitly changes to service-principal access. Prioritize untangling page-level side effects, unsafe SQL writes, global mutable Streamlit/session state, and notebook leftovers before adding new features.
