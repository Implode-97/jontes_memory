# Jontes Memory

Obsidian-backed Codex memory vault.

## Layout

- `Short-Term/`: new capture-memory notes waiting for consolidation.
- `Long-Term/`: compact durable memory used by recall-memory.
- `Dreams/`: consolidation logs from dream-memory.
- `Archive/`: processed short-term notes grouped by dream date.
- `skills/`: portable copies of the memory skills.

## Skills

The repo-local skills are copied from `~/.codex/skills` and adjusted to prefer the current memory root when the standard memory folders are present. That makes the skills reusable from this folder after cloning it elsewhere.

To install them into another Codex setup, copy or symlink the skill folders into that machine's `~/.codex/skills/` directory.

`dream-memory` creates a local Git commit after a successful dream run when this folder is a Git worktree. It does not push automatically.
