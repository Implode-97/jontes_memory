# Documentation Preferences

## Repo Documentation

- The user wants duplicate explanations reduced, but not at the cost of losing important operational detail.
- Keep top-level docs light and navigable, while preserving troubleshooting, pipeline behavior, authentication failure modes, and architecture wiring details in the specialized deep docs.
- When simplifying repo docs, avoid collapsing detailed operational pages into broad summaries unless the same actionable detail remains available elsewhere.

## ADRs And Plans

- When the user asks for ADR or plan text, produce polished normal markdown in chat or a deliberate `.md` file, not a raw markdown dump unless they explicitly ask for one.
- Include only useful sections for the decision, such as Context and Problem Statement, Decision, Consequences, Decision Drivers, Considered Options, Additional Information, Notes, Definition of Done, or Mermaid diagrams when they help review.
- For shareable docs, point implementation references at GitLab or stable project references instead of leaking local-only paths.
