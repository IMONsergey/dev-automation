# dev-automation

Central reusable GitHub Actions foundation for `IMONsergey/*` projects.

## Goals

- GitHub is the source of truth.
- Every change is validated before merge.
- CI behavior is shared instead of copied between repositories.
- ChatGPT/Codex can inspect workflow runs, jobs, logs, and artifacts through the GitHub connector.
- Browser QA, security checks, deployment smoke tests, and release automation can be layered on consistently.

## Included workflows

| Workflow | Purpose |
| --- | --- |
| `reusable-node-ci.yml` | Install dependencies, lint, typecheck, test, build, upload build output |
| `reusable-playwright.yml` | Run Playwright E2E tests and retain reports/traces |
| `reusable-codeql.yml` | CodeQL analysis for supported repositories |
| `reusable-smoke.yml` | HTTP smoke check for preview/production URLs |

## Minimal consumer

Create `.github/workflows/ci.yml` in a project:

```yaml
name: CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  ci:
    uses: IMONsergey/dev-automation/.github/workflows/reusable-node-ci.yml@v1
    with:
      package-manager: npm
      node-version: "22"
```

For maximum reproducibility, production repositories may pin the reusable workflow to a commit SHA instead of `@v1`.

See [`docs/ADOPTION.md`](docs/ADOPTION.md) for rollout guidance.

## Versioning

- `main` is the development line.
- `v1` is the stable compatibility branch for first-generation callers.
- Breaking workflow-input changes require a new major compatibility branch (`v2`, etc.).

## Security model

This repository contains workflow definitions only. Do not store service tokens, API keys, production credentials, or project secrets here. Secrets belong in the calling repository/environment and are passed explicitly only when a workflow requires them.

## Roadmap

1. Shared Node CI
2. Playwright browser QA
3. CodeQL
4. Deployment smoke checks
5. Lighthouse/performance gates
6. Vercel preview orchestration
7. Release workflows
8. PostHog/Sentry feedback loops
