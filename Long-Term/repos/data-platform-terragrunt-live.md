---
scope: repo
repo: data-platform-terragrunt-live
topics: [terragrunt-live, targets, child-pipelines, selectors, providers, yaml]
stability: mixed
last-reviewed: 2026-07-10
---

# data-platform-terragrunt-live

## Repository And Target Shape

- Keep the root CI file as a compact include index with explicit workflow rules and visible bootstrap, validation, target-preparation, and plan stages.
- Keep the active target catalog flat when `stack_dir`, `display_name`, and `kind` make generated labels and variables clear.
- Use one canonical target/account key or a documented composite key; avoid ambiguous repeated names such as `dbcore` across regions.
- Treat `_common_vars.hcl` as a contract with generated units and provider parents, not a constants bag.
- Keep `_account.hcl` selectors explicit: choose a canonical target key and pass only locals consumed by units and provider parents.

## Pipeline Generation

- Use one tested path-matching model, structured target parsing, explicit dependency declarations or real dependency-path parsing, and fail-closed no-match behavior.
- Generated child pipelines should stay small: plan gate, manual deploy gate, resource locking, and current status mirroring.
- Document parent-supplied variables near consumers and avoid stale `strategy: depend` wiring.
- GitLab clone authentication and Terraform registry authentication are separate; preserve both token conventions and document precedence.
- Validation jobs should show the real command and use explicit `changes` gates with a final `when: never` fallback.

## Provider Parents And Stack Runners

- Keep provider parents only when they have visible consumers and one generated provider contract.
- Remove dormant alternate modes, ambient credential paths without a use case, and files competing for the same generated output.
- Keep target runners thin around `terragrunt stack run`; extra generation or filesystem fan-out needs a documented reason and bounded blast radius.

## Current State — Verify

- Verify target inventory, active units, backend configuration, generated child-pipeline shape, and current GitLab status-mirroring behavior before editing live routing.
