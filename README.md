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

Projects without a committed dependency lockfile can preserve an intentional non-frozen install strategy explicitly:

```yaml
      install-command: npm install --no-audit --no-fund
```

The default remains the package manager's frozen/reproducible install (`npm ci`, `pnpm --frozen-lockfile`, `yarn --immutable`, or Bun frozen lockfile). The override exists for imported/prototype repositories while they are being normalized.

For maximum reproducibility, production repositories may pin the reusable workflow to a commit SHA instead of `@v1`.

See [`docs/ADOPTION.md`](docs/ADOPTION.md) for rollout guidance.

## Validated consumers

### BAEV Website

`IMONsergey/BAEV-WEBSITE` is the first production pilot.

Validated behavior:

- private repository calling the public reusable workflow through `@v1`;
- lockfile-less npm install through `install-command`;
- successful Vite production build;
- `dist` retained as a GitHub Actions artifact;
- project-specific entry-point regression guard retained locally;
- Vercel production HTTP smoke check through the shared smoke workflow;
- scheduled production availability checks every six hours.

## Versioning

- `main` is the development line.
- `v1` is the stable compatibility branch for first-generation callers.
- Backward-compatible inputs and bug fixes may advance `v1`.
- Breaking workflow-input changes require a new major compatibility branch (`v2`, etc.).

## Security model

This repository contains workflow definitions only. Do not store service tokens, API keys, production credentials, or project secrets here. Secrets belong in the calling repository/environment and are passed explicitly only when a workflow requires them.

## Roadmap

1. Shared Node CI — live
2. Deployment smoke checks — live
3. Playwright browser QA
4. CodeQL rollout where available
5. Lighthouse/performance gates
6. Vercel preview orchestration
7. Release workflows
8. PostHog/Sentry feedback loops
