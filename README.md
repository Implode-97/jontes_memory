# Jontes Memory

Obsidian-backed Codex memory vault.

## Layout

- `Short-Term/`: new capture-memory notes waiting for consolidation.
- `Long-Term/`: scoped durable memory with an `index.md` router plus `global/`, `domains/`, `repos/`, and `projects/`.
- `Dreams/`: consolidation logs from dream-memory.
- `Archive/`: processed short-term notes grouped by dream date.
- `skills/`: portable copies of the memory skills.

## Skills

The repo-local skills are portable counterparts to the user-wide skills under `~/.codex/skills`. The `recall-memory` and `dream-memory` copies are kept byte-for-byte synchronized; both resolve the current directory as the memory root when the standard folders are present, then fall back to the canonical vault path.

To install them into another Codex setup, copy or symlink the skill folders into that machine's `~/.codex/skills/` directory.

`recall-memory` reads `Long-Term/index.md`, routes by repo/project/domain, and searches only a few candidate files before broadening.

`dream-memory` preserves the scoped long-term structure. In the canonical repository it commits and pushes successful dream runs to `origin/main`; other Git roots commit locally and do not push unless requested.
