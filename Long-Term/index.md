---
scope: index
topics: [memory-routing, recall, aliases]
stability: durable
last-reviewed: 2026-07-15
---

# Long-Term Memory Index

Route by scope before searching by keyword. Read the smallest relevant files; search the full corpus only as a fallback.

## Repository Routing

- `data-platform-terraform-modules`, Terraform modules, wrappers, publishing, registry packages, real provider tests: `repos/data-platform-terraform-modules.md`, then `global/terraform.md` and the relevant Databricks domain file.
- `data-platform-terragrunt-catalog`, catalog, generated stack, platform E2E, guards, `dbx_*`: `repos/data-platform-terragrunt-catalog.md`, then `global/terragrunt.md` and the relevant Databricks domain file.
- `data-platform-terragrunt-live`, live targets, target catalog, child pipelines, `_account.hcl`, `_common_vars.hcl`: `repos/data-platform-terragrunt-live.md`, then `global/terragrunt.md`.
- `everything_app`, `refactor_aula`, Innovation Week Streamlit apps: `repos/innovation-week-apps.md`, then `domains/databricks-apps.md`.
- `RAPIDS`, `jnyjc2_explore_rapids`, cuDF Spark: `repos/rapids.md`, then `domains/databricks-compute.md` and `domains/spark.md`.
- CAV-134396, ingestion onboarding, Woodpeckers, PI planning pack: `projects/cav-134396.md`, then `global/jira.md`.

## Domain Routing

- AWS accounts, profiles, caller identity: `global/aws.md`.
- CI investigations, GitLab pipelines, release safety: `global/ci.md`.
- Codex memory, skills, pets, agent workflow, investigation style: `global/codex-workflow.md`.
- Documentation, READMEs, ADRs, plans: `global/docs.md`.
- Git branches, integration branches, linked worktrees: `global/git.md`.
- Jira stories, Need fields, Firecrackers loop, sprint summaries: `global/jira.md`.
- Terraform module boundaries, inputs, outputs, providers, tests: `global/terraform.md`.
- Terragrunt dependency evaluation, generated files, lifecycle guards, Terratest: `global/terragrunt.md`.
- Databricks identities, workspace assignment, admin groups, platform governance: `domains/databricks-governance.md`.
- Unity Catalog, catalogs, external locations, volumes, bindings, storage: `domains/databricks-unity-catalog.md`.
- Databricks compute, policies, tags, Spark I/O, networking, serverless GPU: `domains/databricks-compute.md`.
- Databricks Apps, user pass-through, app service principals, agent skills: `domains/databricks-apps.md`.
- Spark releases and version watch list: `domains/spark.md`.

## Freshness Rules

- `stability: durable`: use as a preference or convention, while still checking current code when implementation facts matter.
- `stability: mixed`: durable rules plus a `Current State — Verify` section. Verify dated IDs, versions, branches, and deployment facts before acting.
- `stability: verify-current`: treat the file as a routing aid and dated snapshot, not authority.
- Search `Short-Term` separately after long-term recall. A newer explicit correction can supersede long-term memory.
- Historical incidents belong in `Dreams` and `Archive`; do not load them during ordinary recall unless current memory is insufficient.

## Canonical Ownership

- Keep platform semantics in `domains/`, reusable implementation preferences in `global/`, and repository wiring in `repos/`.
- Do not duplicate a rule across scopes. Repo files may name the domain contract they implement, but the detailed rule should have one owner.
- Keep one decision per bullet. Put volatile evidence under `Current State — Verify`, not inside durable rules.
