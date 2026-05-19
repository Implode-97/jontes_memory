# Jira Workflows

## Data Platform Story Creation

- For CAV Data Platform stories, use the `jira-data-platform-story` skill when available.
- Default to project `CAV`, issue type `Story`, component `Sys.Firecrackers`, and the Firecrackers epic such as `CAV-130873` when the user has not overridden them.
- Resolve sprint, component, product owner, and other mandatory values from Jira metadata and known-good reference stories such as `CAV-131342` rather than guessing.
- Keep the Jira description concise and readable; place detailed implementation context in the Need field. Do not paste raw markdown plans directly into Jira fields.

## Firecrackers Epic Loop

- When the user invokes the Firecrackers epic loop, inspect Jira epic `CAV-130873` and prioritize stories whose summaries begin with IaC or are clearly Data Platform IaC work.
- Choose next work from Jira backlog, in-progress, and review state, using story descriptions and Need fields as primary context before local files.
- Inspect relevant local repos, implement or review the work, run focused tests, open or update GitLab merge requests, and only comment or transition Jira when the work state actually changed.

## Sprint Review Summaries

- For sprint review prep, summarize Jira work as concise ticket-first bullets: Jira ID, short title or plain-English summary, and effect-focused notes about what was included or delivered.
- Prefer resolved stories in the sprint or date window when the user asks for work that was done.
- Use Jira `currentUser()` plus the relevant sprint or date window, then focus on `resolutiondate` or status transitions to identify delivered work.
