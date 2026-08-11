# Adoption

## Phase 1 — baseline CI

Add a caller workflow to each Node-based repository:

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

Supported package managers: `npm`, `pnpm`, `yarn`, `bun`.

Optional script-name inputs:

- `lint-script` (default `lint`)
- `typecheck-script` (default `typecheck`)
- `test-script` (default `test`)
- `build-script` (default `build`)

If a named script is absent from `package.json`, it is skipped. Set an input to an empty string to disable that stage explicitly.

For pnpm callers, `pnpm-version` defaults to `10` and can be overridden per project.

## Phase 2 — Playwright

Once a repository has Playwright tests:

```yaml
  e2e:
    uses: IMONsergey/dev-automation/.github/workflows/reusable-playwright.yml@v1
    with:
      package-manager: npm
      node-version: "22"
```

The workflow uploads `playwright-report` and `test-results` even when tests fail.

## Phase 3 — CodeQL

For repositories where GitHub Code Scanning is available:

```yaml
  codeql:
    uses: IMONsergey/dev-automation/.github/workflows/reusable-codeql.yml@v1
    with:
      languages: '["javascript-typescript"]'
```

## Phase 4 — deployment smoke

Use after a deployment job that exposes a URL:

```yaml
  smoke:
    uses: IMONsergey/dev-automation/.github/workflows/reusable-smoke.yml@v1
    with:
      url: ${{ needs.deploy.outputs.url }}
```

## Repository rules

After CI is stable, protect `main` with required status checks and pull requests. Do not make a check required before verifying that every active repository can actually produce that check.
