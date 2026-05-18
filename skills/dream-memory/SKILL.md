---
name: dream-memory
description: Consolidate Obsidian Codex short-term memory into compact long-term memory during explicit dream runs or scheduled automation only. Use when the user asks to dream, consolidate, process, archive, or maintain memory, or when invoked by a cron automation. Do not use during ordinary conversations, normal recall, or short-term capture.
---

# Dream Memory

Run dream consolidation as a separate maintenance action. Do not trigger this during normal user conversations unless the user explicitly asks for it or a scheduled automation invokes it.

## Paths

Resolve the memory root before starting:

- Prefer the current working directory when it contains `Short-Term`, `Long-Term`, `Dreams`, and `Archive`.
- Otherwise use the default memory root:

`/Users/jnyjc2/Documents/Obsidian Vault/CodexMemory/CodexMemory/Memory`

Use these folders:

- `Short-Term`: input notes to process.
- `Long-Term`: compact durable memory, organized by category files.
- `Dreams`: dream logs and clarification notes.
- `Archive`: processed short-term notes, preserving original filenames under a dated run folder.

## Run Modes

Interactive run: if an unresolved conflict appears, pause and ask the user before updating long-term memory or archiving affected notes.

Scheduled run: never wait for chat input. Process non-conflicting notes, leave conflicted notes in `Short-Term`, and write a `Needs Clarification` section in the dream log.

## Empty Short-Term

If `Short-Term` contains no `.md` files, exit successfully without creating a new dream log. For scheduled runs, optionally update `Dreams/last-run.md` if observability is needed, but do not create log spam.

## Workflow

1. Snapshot all `Short-Term/*.md` files at run start.
2. Parse each note's frontmatter, title, `Context`, `Learning`, and `Future Use`.
3. Cluster notes by meaning and future use, not only by filename topic.
4. Search matching `Long-Term` files before deciding promotion.
5. Search `Archive` for recurrence before discarding low-value or duplicate notes.
6. Decide each cluster outcome: promote, merge, already-known, ignore, conflict, or needs-clarification.
7. Create, update, compact, split, or rename long-term files as needed.
8. Write one dream log describing all decisions and any long-term refactors.
9. Archive only files that were fully processed after long-term updates and the dream log are written.
10. If the memory root is a Git worktree, create a local commit for the completed dream run.

## Long-Term Shape

Prefer one compact file per category at first, such as `jira.md`, `terragrunt.md`, `ci.md`, `docs.md`, or `codex-workflow.md`.

Long-term files may be refactored. Split a broad file when it becomes mixed, too large to scan, or hard to recall by filename. For example, split `jira.md` into `jira-formatting.md`, `jira-story-creation.md`, and `jira-sprint-review.md`, then keep `jira.md` as a short index if useful.

Dreaming may rewrite, compact, rename, and split long-term files. It is not append-only. Preserve meaning, remove duplication, and log every refactor.

## Importance Model

Use gates before scoring. Promote only memories that change future behavior.

Always consider promotion when a learning contains:

- User preference or decision.
- Durable project, repo, tool, or domain convention.
- Correction of a previous assumption.
- Repeated pattern found in short-term or archive.
- High cost of forgetting.
- Actionable future-use guidance.

Estimate cost of forgetting dynamically from context:

`consequence x recurrence_likelihood x rediscovery_cost x visibility`

Examples:

- Jira formatting mistakes are usually worth long-term memory because they are externally visible, recurring, and waste cleanup time.
- Repo-specific CI constraints are usually worth long-term memory because forgetting them can break pipelines repeatedly.
- A one-off command failure is not worth long-term memory unless it reveals a durable environment rule or recurs in archive.

Ignore or archive without promotion when a note is only transient task status, already fully captured in long-term memory, vague, non-actionable, or tied to a temporary incident.

## Existing Memory And Missed Recall

If a short-term note duplicates something already present in long-term memory, do not duplicate it. Log it under `Already Known`.

If the duplicate looks like Codex should have remembered it during the original conversation, also log it as a `Potential Recall Miss` with the long-term file and rule that should have been found. This is a signal to improve `recall-memory` or long-term wording.

## Conflict Handling

Resolve conflicts in this order:

1. Explicit correction: a newer note clearly corrects or supersedes an older memory.
2. Special case: both memories can be true under different repo, tool, date, scope, or condition.
3. Evidence: current repo state, Jira, docs, or tool behavior clearly supports one side.
4. Ask or defer: if still ambiguous, do not guess.

Interactive runs should ask the user at step 4. Scheduled runs should leave affected notes in `Short-Term`, write `Needs Clarification`, and archive only non-conflicting processed notes.

Never write unresolved conflict into long-term memory as truth.

## Dream Log

Create logs in `Dreams` named:

`dream-YYYYMMDD-HHMMSS.md`

Include these sections when relevant:

```markdown
# Dream: YYYY-MM-DD HH:MM:SS

## Summary

## Promoted

## Merged

## Already Known

## Potential Recall Misses

## Ignored

## Conflicts

## Needs Clarification

## Refactored Long-Term Memory

## Archived

## Left In Short-Term
```

## Archive Rules

Move processed short-term files to:

`Archive/YYYYMMDD/`

Preserve original filenames. Archive only after long-term writes and dream log writes succeed. Leave unprocessed or unresolved-conflict files in `Short-Term` and list them in the dream log.

## Git Commit

After a successful dream run, check whether the memory root is inside a Git worktree:

```bash
git rev-parse --is-inside-work-tree
```

If it is not a Git worktree, skip this step. If it is a Git worktree, run `git status --short` from the memory root. When there are no changes, exit successfully without creating an empty commit.

When there are changes:

1. Stage the completed memory changes with `git add -A`.
2. Create one local commit.
3. Use this commit message shape:

```text
dream YYYY-MM-DD: short relevant phrase
```

Choose a short phrase from the most important promoted, merged, archived, or refactored topic in the dream log. Prefer concrete phrases such as `jira formatting cleanup`, `terragrunt test notes`, or `codex workflow polish`. If no specific topic stands out, use `memory consolidation`.

Do not push automatically unless the user explicitly asked for pushing in the current run or automation prompt.
