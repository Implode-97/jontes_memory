---
scope: global
topics: [codex, memory, skills, investigation, planning, agent-security]
stability: mixed
last-reviewed: 2026-07-10
---

# Codex Workflow

## Memory System

- Use `recall-memory` before work that may depend on prior preferences, decisions, or project context.
- Use `capture-memory` only for reusable learnings; it writes concise notes to `Short-Term` and does not consolidate long-term memory.
- Dream runs consolidate `Short-Term`, update scoped long-term files, write one log, and archive fully processed notes.
- Scheduled dreams never wait for input. They leave unresolved conflicts in `Short-Term` with `Needs Clarification` in the log.

## Memory Repository

- The canonical memory root is a Git repository on `main` with origin `https://github.com/Implode-97/jontes_memory.git`.
- Portable skill copies live under `skills/` and should stay synchronized with their global counterparts.
- Successful canonical dream runs create one dated commit and push `origin/main`; non-canonical roots do not push unless requested.

## Investigation And Planning

- Trace bugs from full logs, code, and configuration to the earliest failing operation and underlying contract.
- Before changing Terraform or Terragrunt, compare fixes with existing ownership boundaries and prefer extending a natural boundary over adding a new one.
- Flag empty tracked governance files such as `CODEOWNERS` unless their emptiness is intentional and documented.
- For substantial IaC plans, combine recalled memory, current code, official sources, `iac-code-style`, decision grilling when useful, and independent domain/security/test/KISS review.
- Produce evidence-backed decisions, capture durable learnings, and create handoffs at phase boundaries or blockers.

## Skill Authoring

- Skills must work from a cold start: state purpose, inputs, source of truth, workflow, decision rules, and expected output.
- Keep trigger-rich frontmatter, concise bodies, explicit destructive-action guardrails, and small examples. Mark live tests and diagnostic outputs clearly.
- Repo-local skills are discovered from `.agents/skills`, not a top-level `skills/` directory.
- Use the canonical user-wide skills when applicable: `ci-log-triage`, `jira-mcp-formatting`, `score-code-with-agents`, and `handoff` under `/Users/jnyjc2/.codex/skills/`.
- If newly installed or edited skill metadata does not appear, restart Codex or open a new task before rewriting the skill.

## Agent Tool Security

- Treat local agent harnesses such as Omnigent as configurable persistence and forwarding systems, not inherently as Databricks SaaS.
- Start trials locally without sensitive repositories. Minimize tracing, content capture, environment passthrough, and provider retention; require auth for shared deployments.
