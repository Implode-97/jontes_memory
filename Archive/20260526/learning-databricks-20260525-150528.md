---
type: short-term-learning
topic: databricks
created: 2026-05-25 15:05:28
source: codex-conversation
confidence: high
---

# Learning: Sandbox Wrapper Real Test Workspace

## Context
While fixing the modules repo MR CI for the sandbox catalog wrapper real apply test, the required workspace variables were missing because the dynamic module runner executes `terraform test` without `-var` or `TF_VAR` wiring.

## Learning
The shared dev workspace for the sandbox catalog wrapper real test is `https://dbc-b72ad987-f679.cloud.databricks.com` with workspace ID `4022911453310458`. The Databricks account API showed it belongs to account `47dc11d5-0e29-4a29-b9b0-9d3d3961e518`, runs in `eu-west-1`, and its current metastore is `3f86dad4-0a1b-4efb-9bcd-e32a1a541300`.

## Future Use
For this module's real apply test, prefer hard-coded test defaults for these shared environment facts instead of requiring CI-level `TF_VAR_databricks_workspace_host` and `TF_VAR_databricks_workspace_id`. This keeps module CI self-contained and avoids silent test-file load failures.
