---
scope: global
topics: [documentation, readme, adr, plans, ci-docs]
stability: durable
last-reviewed: 2026-07-10
---

# Documentation Preferences

## Repository Documentation

- Reduce duplicate explanations without deleting operational detail. Keep top-level docs navigable and specialized docs authoritative.
- Root READMEs are maps: purpose, layout, primary locations, and a small quick start.
- Module READMEs describe stable behavior, inputs, provider expectations, validation, and prerequisites—not recent implementation progress.
- Test commands must match actual `.tftest.hcl` discovery. Label live/shared-resource commands with credentials, fixtures, and cleanup caveats.
- CI docs must match active includes, stages, jobs, artifacts, branch behavior, and exclusions. Keep source gating in one authoritative place.
- Release docs own durable tag formats, stable versus RC behavior, bump precedence, active-RC handling, manual intent, and remote tag history.
- Distinguish required inputs from CI defaults, active checks from planned coverage, and stable vocabulary from volatile inventories.
- Databricks module docs should state provider scope, account/workspace boundaries, bindings, and authentication without becoming platform tutorials.
- Terraform docs should separate stable public outputs from diagnostic or test-only composites.
- Lifecycle-managed Terragrunt docs should explain states, staged destroy, row removal, guard intent, prerequisites, and operator checkpoints once.
- Verify high-blast-radius live IaC claims against current target inventory, CI, generated pipelines, stack units, and backend configuration.

## ADRs And Plans

- Produce polished Markdown in chat or a deliberate file; do not emit a raw Markdown dump unless requested.
- Include only decision-useful sections such as context, decision, consequences, drivers, options, notes, definition of done, and diagrams when useful.
- Shareable documents should point to GitLab or stable project references, not local-only paths.
