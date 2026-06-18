---
type: short-term-learning
topic: databricks
created: 2026-06-17 10:31:24
source: codex-conversation
confidence: high
---

# Learning: refactor_aula now has aula_app page and repository layers

## Context
During implementation in `/Users/jnyjc2/code/innovation-week/refactor_aula`, concurrent edits replaced legacy Streamlit pages with thin `Pages/page_*.py` wrappers that route into `aula_app.ui` and `aula_app.pages`.

## Learning
Future work should treat `aula_app/pages`, `aula_app/repositories.py`, `aula_app/services.py`, `aula_app/files.py`, and `aula_app/models.py` as the active greenfield refactor surface. The current pass-through contract is enforced by user-token SQL connector calls, explicit Files API bearer-token access, typed `AnnotationRow` validation, allowlisted Unity Catalog table identifiers, and `ImageKey` identities that include `sequence_id` plus `camera_id`.

## Future Use
Do not resurrect page-level annotation SQL or sequence-only image maps from `Pages/` and `Utilities/`. When adapting UI behavior, preserve the merged `AulaRepository` and service helper APIs that the new page modules import, and keep DAB/resource changes separate unless explicitly requested.
