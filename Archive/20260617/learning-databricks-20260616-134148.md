---
type: short-term-learning
topic: databricks
created: 2026-06-16 13:41:48
source: codex-conversation
confidence: high
---

# Learning: Databricks Agent Skills for Apps PoCs

## Context
While acting as a read-only subagent for `/Users/jnyjc2/code/innovation-week/everything_app`, the Databricks `databricks-agent-skills` repository was cloned and inspected for Databricks Apps PoC guidance.

## Learning
The stable repo skills are AppKit-first: use `databricks-core`, `databricks-apps`, `databricks-app-design`, and `databricks-dabs` as the main guidance. The most useful app references are `platform-guide.md`, `other-frameworks.md`, AppKit `overview.md`, `sql-queries.md`, `frontend.md`, `testing.md`, `files.md`, and `custom-endpoints.md`. Experimental skills add useful Python, SDK, and Unity Catalog snippets, especially for Streamlit/FastAPI auth, `valueFrom` resources, UC Volume file operations, and synchronous SDK calls inside async apps.

## Future Use
For future Databricks Apps work, prefer installing the Databricks Codex plugin or CLI-installed skills globally instead of vendoring the full repo into an app project. Only import the experimental Python/SDK/UC skills if the project is explicitly Python-first or agents lack those skills.
