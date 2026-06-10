# Documentation Preferences

## Repo Documentation

- The user wants duplicate explanations reduced, but not at the cost of losing important operational detail.
- Keep top-level docs light and navigable, while preserving troubleshooting, pipeline behavior, authentication failure modes, and architecture wiring details in the specialized deep docs.
- When simplifying repo docs, avoid collapsing detailed operational pages into broad summaries unless the same actionable detail remains available elsewhere.
- Module READMEs should describe stable behavior, input contracts, provider expectations, validation commands, and durable prerequisites. Avoid development-log wording about current test status, recent moves, or one-off implementation progress; frame test details as validation prerequisites or environment facts when they must remain.
- Root READMEs should still orient a new contributor: short purpose, layout, primary code/docs locations, and a tiny quick start. Treat root READMEs as maps, not second sources of truth for detailed CI, release, E2E, troubleshooting, or local debug behavior.
- README test commands must match actual `.tftest.hcl` files and CI discovery. If live provider tests live under default `tests/`, default local commands should use mock-safe `-filter` examples or clearly label the command as live/shared-resource behavior with credentials, fixtures, and cleanup caveats.
- CI docs must track the real root `.gitlab-ci.yml`, includes, stages, jobs, artifacts, and intentional exclusions. Prefer one authoritative source-gating section and link to specialized docs instead of repeating policy across the README.
- CI/CD docs should be compact maps of stages, jobs, branch behavior, and release gates. Include non-blocking review lanes when they exist, but let specialized docs own E2E execution, OIDC setup, release semantics, troubleshooting, and local debug details.
- Release docs should own durable release behavior when CI creates persistent tags: unit-scoped tag format, default-branch stable releases, non-default RC releases, bump precedence, active RC mismatch handling, manual release intent, and remote tag history as source of truth.
- Contributor, auth, testing, and naming docs must distinguish required user inputs from CI defaults, active checks from planned coverage, and stable vocabulary from volatile inventories. Compare local commands, variable lists, and proof claims against included CI jobs and uncommented test code before accepting them.
- Databricks module READMEs should keep provider scope, account-vs-workspace boundaries, workspace binding ownership, and pass-through/auth facts concise and explicit. Do not let small leaf modules grow into Unity Catalog or Databricks entitlement tutorials.
- For Terraform module docs, identify stable downstream outputs separately from diagnostic composites or test-only outputs so future consumers do not mistake temporary convenience for public API.
- Terragrunt unit READMEs for lifecycle-managed IaC should keep the state table, staged destroy or tombstone rules, row-removal rules, guard intent, prerequisites, and operator checkpoints explicit. Avoid repeating the same state machine as multiple diagrams/tables, long mock-output blocks, or local credential transcripts; separate local guard tests from active shared E2E coverage.
- Live IaC README claims have high blast radius. Compare target inventory, CI stages, generated child-pipeline shape, active stack units, and root backend config against current repo state; stale backend, remote-state, target, or coverage claims are rework-level issues even when prose is otherwise concise.

## ADRs And Plans

- When the user asks for ADR or plan text, produce polished normal markdown in chat or a deliberate `.md` file, not a raw markdown dump unless they explicitly ask for one.
- Include only useful sections for the decision, such as Context and Problem Statement, Decision, Consequences, Decision Drivers, Considered Options, Additional Information, Notes, Definition of Done, or Mermaid diagrams when they help review.
- For shareable docs, point implementation references at GitLab or stable project references instead of leaking local-only paths.
