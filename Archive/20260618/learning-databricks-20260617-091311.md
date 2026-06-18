---
type: short-term-learning
topic: databricks
created: 2026-06-17 09:13:11
source: codex-conversation
confidence: high
---

# Learning: Databricks App Streamlit Selection Pattern

## Context
In the `everything_app` Databricks App PoC, the user wanted Unity Catalog exploration to avoid manual copy/paste between listed schemas, tables, and input boxes. The app remains a pass-through-only proof of concept with no SQL warehouse, Unity Catalog, or volume resources granted to the app service principal.

## Learning
For this Streamlit app, use `st.dataframe` row selection with stable widget keys and session-state-backed text inputs to make catalog object browsing interactive. Selecting a catalog can populate catalog inputs and clear dependent schema/table/path fields; selecting a schema populates the schema input and clears the table; selecting a table populates the table input; selecting a volume can build `/Volumes/{catalog}/{schema}/{volume}`. This keeps the app KISS without switching frameworks.

## Future Use
When extending this project, preserve the pass-through-only boundary and prefer this selectable-dataframe pattern before proposing a React/AppKit fork. Consider AppKit only when richer client-side state, multi-pane navigation, or heavier interactivity becomes a real product requirement.
