---
type: short-term-learning
topic: databricks
created: 2026-06-17 14:09:53
source: codex-conversation
confidence: high
---

# Learning: refactor_aula Streamlit pages need explicit URL paths

## Context
While testing the deployed `/Users/jnyjc2/code/innovation-week/refactor_aula` Databricks Streamlit app, Streamlit raised `Multiple Pages specified with URL pathname render` on first page load.

## Learning
The greenfield app uses `st.Page(start.render)`, `st.Page(owl.render)`, and `st.Page(clip.render)`. Because each page callable is named `render`, Streamlit infers the same `render` URL pathname unless `url_path` is set explicitly. The fix is to give every page a stable path such as `start`, `owl-bbox-labels`, and `clip-image-labels`, and to keep a unit test that asserts those paths are unique.

## Future Use
When adding or refactoring pages in this app, do not rely on callable-name inference if modules share a `render` function. Set explicit `url_path` values and browser-check the routes after deployment when authentication permits.
