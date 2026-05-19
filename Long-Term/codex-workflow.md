# Codex Workflow Memory

## Obsidian Memory System

- Normal sessions should use `recall-memory` before relevant work and `capture-memory` only when a reusable learning appears.
- `capture-memory` creates concise short-term learning notes; it should not perform long-term consolidation during ordinary work.
- Dream runs are separate maintenance actions that consolidate `Short-Term` notes into compact long-term category files, archive processed notes, write one dream log, and avoid creating noise when there is nothing to process.
- Interactive dream runs may ask for clarification on unresolved conflicts. Scheduled dream runs must not wait for input; they should process non-conflicting notes and leave ambiguous notes in `Short-Term` with a `Needs Clarification` log section.

## Memory Git Repository

- The Obsidian Codex memory root is a Git repository on branch `main` with origin `https://github.com/Implode-97/jontes_memory.git`.
- Repo-local portable copies of `recall-memory`, `capture-memory`, and `dream-memory` live under `skills/` so they can be reused or installed elsewhere.
- Successful dream runs should create one local commit with a short dated message, but should not push unless the user explicitly asked for pushing in the current run.
