# Codex Workflow Memory

## Obsidian Memory System

- Normal sessions should use `recall-memory` before relevant work and `capture-memory` only when a reusable learning appears.
- `capture-memory` creates concise short-term learning notes; it should not perform long-term consolidation during ordinary work.
- Dream runs are separate maintenance actions that consolidate `Short-Term` notes into compact long-term category files, archive processed notes, write one dream log, and avoid creating noise when there is nothing to process.
- Interactive dream runs may ask for clarification on unresolved conflicts. Scheduled dream runs must not wait for input; they should process non-conflicting notes and leave ambiguous notes in `Short-Term` with a `Needs Clarification` log section.

## Memory Git Repository

- The Obsidian Codex memory root is a Git repository on branch `main` with origin `https://github.com/Implode-97/jontes_memory.git`.
- Repo-local portable copies of `recall-memory`, `capture-memory`, and `dream-memory` live under `skills/` so they can be reused or installed elsewhere.
- Successful dream runs in the canonical memory root should create one short dated commit and push it to `origin/main`. This repo is explicitly authorized for automatic dream commits and pushes; non-canonical memory roots should still avoid pushing unless the user asks.

## Investigation Workflow

- For bug, CI, and IaC investigations, trace from full logs, code, and configuration to the earliest real failing operation and the underlying contract or permission problem. Do not stop at downstream cleanup errors, ordering symptoms, or the last visible error unless the user explicitly asks for quick triage.
- Before editing Terraform or Terragrunt, compare plausible fixes against existing modules, ownership groups, and lifecycle boundaries. Prefer extending the current ownership model over adding a separate feature boundary when the existing design has a natural place for the fix.

## Skill Authoring

- Skills for this user must be cold-start usable by an agent with no surrounding conversation context. State the purpose, required inputs, source of truth, workflow, decision rules, and expected output explicitly.
