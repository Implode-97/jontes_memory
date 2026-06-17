---
type: short-term-learning
topic: databricks
created: 2026-06-17 08:45:13
source: codex-conversation
confidence: high
---

# Learning: Streamlit Results Should Use Shared Output Space

## Context
In `/Users/jnyjc2/code/innovation-week/everything_app`, the user reported that Databricks App result tables were truncated because `_show_result()` was called inside Streamlit button columns.

## Learning
For this Streamlit Databricks Apps PoC, action buttons can remain in compact columns for scanning, but query results, errors, and dataframes should render after the button row in a shared full-width area. Rendering result tables inside the clicked column makes wide Unity Catalog table previews and table lists nearly unusable.

## Future Use
When adding new actions or tabs to this app, collect the selected action from the button row first, then render `_show_result()` below the columns. Keep `st.dataframe(..., width="stretch", height=600)` for result-heavy views so users have usable horizontal and vertical space.
