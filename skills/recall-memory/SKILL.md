---
name: recall-memory
description: Search scoped Obsidian Codex memory before acting when prior user preferences, project decisions, repository contracts, architecture context, implementation patterns, Terraform/Terragrunt/Databricks conventions, or Codex workflow learnings may matter. Use at the start of tasks that benefit from context across sessions.
---

# Recall Memory

Recall the smallest relevant memory set before recommending, reviewing, or editing.

## Resolve The Memory Root

- Prefer the current working directory when it contains `Short-Term`, `Long-Term`, `Dreams`, and `Archive`.
- Otherwise use `/Users/jnyjc2/Documents/Obsidian Vault/CodexMemory/CodexMemory/Memory`.

Search `Long-Term` and `Short-Term`. Do not search `Dreams` or `Archive` during ordinary recall unless current memory is insufficient.

## Scoped Recall Workflow

1. Identify scope from the request and working directory: repository, project or Jira key, domain, workflow, and exact artifact or error.
2. Read `Long-Term/index.md` when present. Use its aliases and routing to select likely files.
3. Discover candidates by filename and content before printing matches:

```bash
rg -il --glob '*.md' "repo-name|project-key|specific-term|error-token" Long-Term Short-Term
```

4. Choose the best one to three long-term files. Prefer an exact repo or project file, then the relevant domain and global preference files.
5. Search only those candidates with narrow terms and small context:

```bash
rg -n -i -C 1 "specific-term|related-term" <candidate-files>
```

6. Read the relevant heading or section, not the whole corpus.
7. Search `Short-Term` separately for newer corrections or unresolved context:

```bash
rg -n -i -C 1 --glob '*.md' "specific-term|related-term" Short-Term
```

8. Broaden to all memory files only when scoped recall finds nothing. Avoid starting with generic terms such as `databricks`, `terraform`, `workspace`, or `ci` alone.

If Obsidian search is more efficient, use it only after selecting similarly narrow terms:

```bash
obsidian vault="CodexMemory" search query="specific term" limit=10
```

## Scope And Freshness

- `global/`: reusable user preferences and implementation conventions.
- `domains/`: platform or technology semantics.
- `repos/`: repository wiring, contracts, and current implementation state.
- `projects/`: active initiative or Jira context.
- `stability: durable`: use as a convention, while checking current code when implementation details matter.
- `stability: mixed` or `verify-current`: verify dated IDs, versions, branches, APIs, and deployment facts before acting.
- A newer explicit correction in `Short-Term` can supersede long-term memory.

## Use

Summarize only the relevant points in your own words. Treat memory as context, not authority. Follow the current user request when it conflicts with memory and mention the conflict briefly.

Keep recall read-only. Do not consolidate, rewrite, move, archive, or create memory while using this skill.
