# Jira Workflows

## Data Platform Story Creation

- For CAV Data Platform stories, use the `jira-data-platform-story` skill when available.
- Default to project `CAV`, issue type `Story`, component `Sys.Firecrackers`, and the Firecrackers epic such as `CAV-130873` when the user has not overridden them.
- Resolve sprint, component, product owner, and other mandatory values from Jira metadata and known-good reference stories such as `CAV-131342` rather than guessing.
- Keep the Jira description concise and readable; place detailed implementation context in the Need field. Do not paste raw markdown plans directly into Jira fields.
- For the CAV Story Need custom field, `customfield_11317`, write Jira wiki-style text directly through `additionalFields`, such as `h2. Context` headings and `#` ordered-list lines, then verify read-back. Descriptions and comments should still use the Jira MCP formatting skill's safe Markdown path; raw Jira `#` list markers in comments can be converted as headings by the MCP comment converter.
- For child work under a CAV Story, use issue type `Technical task` with the required `parent` field set to the parent Story key, usually via `customFields.parent = {"key": "<story-key>"}` in Jira tool calls. Do not set Epic Link on that child task because it inherits the epic association through the parent Story; the review transition name is `Review`. If Jira assignee lookup by email is rejected in CAV, use username `jnyjc2` unless a concrete account ID is available.

## Firecrackers Epic Loop

- When the user invokes the Firecrackers epic loop, inspect Jira epic `CAV-130873` and prioritize stories whose summaries begin with IaC or are clearly Data Platform IaC work.
- Choose next work from Jira backlog, in-progress, and review state, using story descriptions and Need fields as primary context before local files.
- Inspect relevant local repos, implement or review the work, run focused tests, open or update GitLab merge requests, and only comment or transition Jira when the work state actually changed.

## CAV-134396 Ingestion Team Onboarding

- `CAV-134396` is the PI2616 successor-style epic for Data Foundation ingestion-team onboarding. It builds on `CAV-130873` and shifts Databricks dev workspace work toward governed, repeatable onboarding with external locations/storage credentials, SQL warehouse usage, instance profile posture, guardrails, user-facing roles, cost tagging, workspace settings, TOOCAN validation, and one end-to-end Woodpeckers onboarding flow.
- As of 2026-06-22, the local `sandbox_catalog/epic` placeholders were promoted into Jira child Stories `CAV-134403` through `CAV-134418`, covering Stories 01, 02, 02a, 03, 03a, 03b, 03c, and 04 through 12. They use project `CAV`, issue type `Story`, component `Sys.Firecrackers`, product owner `spahj5`, labels `cav-134396` and `pi-planning-pack`, Epic Link `CAV-134396`, and intentionally no live Jira Sprint because the markdown used relative planning buckets.
- Future CAV-134396 work should start from the live Jira child stories, current repo state, handoffs, `epic/README.md`, and `epic/00-epic-goal-clarifications.md`. The only old root implementation plan to treat as fully relevant is `external-location-feature-plan.md`; other old root plans are historical/debt context unless a story explicitly revalidates them.
- Treat the `epic/` folder as a PO-presentable decision cockpit, not final implementation tickets. Unresolved TOOCAN, SQL topology, instance profile, source-side AWS ownership, and Woodpeckers-input decisions are acceptable when visible with owners, gates, and fallbacks; do not downgrade the pack unless decisions are hidden or evidence has changed.
- Prioritize direct Woodpeckers and ingestion-team onboarding. SQL warehouse and instance profile work should be pure IaC across Terraform modules and Terragrunt catalog repos; workspace assignment belongs in Story 03b; serverless budget policy Story 03c is conditional core only if PO/Woodpeckers requires serverless; cost dashboard is stretch; tagging per feature is core; TOOCAN remains an enablement/compliance decision gate.
- Treat the `CAV-134270` data product branch as PoC/start-work input only until Story 02a approves scope and Story 03 defines any downstream product service-principal contract. Keep team onboarding valid without product service principals and keep product-SP dependencies in workspace assignment, compute, SQL, cost tagging, external locations, budget policy, and TOOCAN validation conditional.
- When re-reviewing the pack, score it as a PO discussion package: provide a per-file score table, arithmetic average across all `.md` files, and an overall pack score. Both the average and overall score must exceed 92 to accept; otherwise list exact limiting files and fixes. The final evidence story should reconcile every central decision and carryover with owner/date/sign-off before closure.

## Sprint Review Summaries

- For sprint review prep, summarize Jira work as concise ticket-first bullets: Jira ID, short title or plain-English summary, and effect-focused notes about what was included or delivered.
- Prefer resolved stories in the sprint or date window when the user asks for work that was done.
- Use Jira `currentUser()` plus the relevant sprint or date window, then focus on `resolutiondate` or status transitions to identify delivered work.
