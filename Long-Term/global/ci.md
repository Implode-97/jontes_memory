---
scope: global
topics: [ci, gitlab, pipelines, releases, investigation]
stability: durable
last-reviewed: 2026-07-10
---

# CI Preferences

## Failure Investigation

- Fetch the authoritative job trace when available; pasted excerpts are fallback evidence.
- Start from the earliest real failure. Later destroy, cleanup, dependency, and output errors are often fallout.
- Use `/Users/jnyjc2/.codex/skills/ci-log-triage` for noisy GitLab CI, Terratest, Terraform, or Terragrunt logs.
- When the pipeline already provides the authoritative result, run local tests only when they answer a specific unresolved question.
- If a test command excludes a package, verify that an active replacement lane covers it. An unused CI job file is not coverage.

## GitLab Review And Child Pipelines

- Compare the MR SHA, source branch head, commit list, and MR pipelines before treating the newest detached pipeline as current.
- Use `trigger: strategy: mirror` when a bridge should mirror downstream status; request artifacts only when the bridge consumes them.
- API helpers must handle pagination, timeouts, bounded logs, and explicit write-capable tokens. Do not assume `CI_JOB_TOKEN` can write MR notes.
- Non-authoritative AI review lanes should stay asynchronous from real validation and run with pinned tooling, narrow environment allowlists, and least-privilege tokens.

## Release Safety

- Serialize persistent release work with `resource_group`, guard against tag-pipeline recursion, and state protected/default-branch assumptions.
- Treat remote tag history and breaking-change markers as safety-critical inputs; fail if tag refresh fails and scan full commit messages for `BREAKING CHANGE` footers.
- Validate package and version inputs before publishing immutable artifacts. Never replace a missing same-pipeline dependency with an unrelated latest stable release.
- Keep manual release `allow_failure` intent explicit.
