---
scope: global
topics: [jira, cav, stories, technical-tasks, firecrackers, sprint-review]
stability: mixed
last-reviewed: 2026-07-10
---

# Jira Workflows

## Data Platform Stories

- Use `jira-data-platform-story` for CAV Data Platform stories when available.
- Default to project `CAV`, issue type `Story`, component `Sys.Firecrackers`, and the relevant Firecrackers epic unless the user overrides them.
- Resolve sprint, component, product owner, and mandatory fields from Jira metadata and known-good stories; do not guess.
- Keep descriptions concise and put implementation detail in the Need field.
- Write CAV Need field `customfield_11317` as Jira wiki text through `additionalFields`, then verify read-back.
- Use the Jira MCP formatting skill for descriptions and comments; raw `#` list markers in comments can be misread as headings.
- Create Story children as `Technical task` with `parent = {"key": "<story-key>"}`. Do not set Epic Link on the child; use transition `Review`.
- If CAV assignee lookup by email fails, use username `jnyjc2` unless a concrete account ID is available.

## Firecrackers Work Loop

- Start from Jira epic `CAV-130873` and prioritize summaries beginning with IaC or clearly belonging to Data Platform IaC.
- Choose work from live Jira state and read descriptions and Need fields before local plans.
- Implement or review in the relevant repositories, run focused tests, and update Jira only when the work state changed.

## Sprint Review Summaries

- Use concise ticket-first bullets: Jira ID, short title, and effect-focused delivered work.
- Prefer resolved issues in the requested sprint or date window.
- Query `currentUser()` and use resolution dates or status transitions to identify delivered work.
