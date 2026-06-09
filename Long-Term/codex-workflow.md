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
- Empty tracked governance files such as root `CODEOWNERS` should be flagged in repo hygiene reviews unless documented as intentional. Prefer deleting placeholders, adding a concise intent comment, or adding the smallest valid rule after owner eligibility is verified.

## Skill Authoring

- Skills for this user must be cold-start usable by an agent with no surrounding conversation context. State the purpose, required inputs, source of truth, workflow, decision rules, and expected output explicitly.
- Repo-local Codex skills are discovered from `.agents/skills`, not from a top-level `skills/` directory. In `/Users/jnyjc2/code/ng-iac`, the `iac-mr-review` skill lives at `.agents/skills/iac-mr-review`; if `$` autocomplete does not show it after reload, restart Codex or open a new thread before changing the skill contents.
- Repo-local skill files score best with concise trigger-rich frontmatter, little body repetition, clear security/destructive-action guardrails, and small concrete examples for validation or naming rules. Label live-provider tests and diagnostic outputs so reviewers do not mistake test convenience for public API.
- The canonical user-scoped multi-agent code scoring skill is `/Users/jnyjc2/.codex/skills/score-code-with-agents/SKILL.md`. Use it when the user asks for `SPAWN`, multi-agent scoring, readability/looks review, KISS/YAGNI assessment, overengineering review, or batch scoring. It should spawn 3-6 independent codebase-relevant agents, pass neutral local context and current primary-doc context when relevant, preserve explicit `xhigh` reasoning requests, stay read-only for score-only reviews, and synthesize 0-100 scores with a practical recommendation.
- A local `handoff` skill exists at `/Users/jnyjc2/.codex/skills/handoff`. Use it when the user asks for a handoff, continuation note, context transfer, session summary, or to prepare the next Codex session; it should produce a concise Markdown handoff file in the OS temporary directory and report that path.
