---
scope: domain
domain: databricks
topics: [databricks-apps, user-pass-through, forwarded-token, appkit, agent-skills]
stability: mixed
last-reviewed: 2026-07-10
---

# Databricks Apps

## Authentication And Data Access

- Default SDK clients such as `WorkspaceClient()` use the app service principal from injected environment variables.
- User pass-through requires the forwarded `x-forwarded-access-token` and an explicitly constructed SDK, REST, or SQL client.
- Trace each SQL, Files API, Unity Catalog, volume, and fallback call to its exact credential source; mixed auth and caches can split principals.
- When user authorization is intended, fail fast instead of silently falling back to the app service principal.
- Do not declare SQL warehouses, tables, or volumes as app resources in a pass-through proof; those grants belong to the app principal.
- Request only required user scopes and never cache per-user forwarded-token clients as global resources.

## Project And Skill Guidance

- Current Apps projects can use `pyproject.toml` plus `uv.lock`; re-check official docs when older vendored skills disagree.
- Prefer the Databricks plugin or global CLI-installed skills for general Apps work.
- When vendoring is requested, copy a focused `.agents/skills/databricks-agent-skills` snapshot and exclude `.agents/**` from bundle sync and linting.
- Start Apps PoCs with AppKit-focused skills: core, apps, design, bundles, SQL, frontend, testing, files, and custom endpoints.

## Current State — Verify

- Re-check accepted user scopes and deployment metadata against the current Apps API; effective metadata and create-time validation have differed.
