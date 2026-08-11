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

Optional inputs:

- `install-command` (default empty; overrides the default dependency install when explicitly set)
- `lint-script` (default `lint`)
- `typecheck-script` (default `typecheck`)
- `test-script` (default `test`)
- `build-script` (default `build`)
- `artifact-path` (default `dist`)
- `artifact-name` (default `build-output`)

If a named script is absent from `package.json`, it is skipped. Set an input to an empty string to disable that stage explicitly.

The default dependency installation is intentionally frozen/reproducible:

- npm: `npm ci`
- pnpm: `pnpm install --frozen-lockfile`
- yarn: `yarn install --immutable`
- bun: `bun install --frozen-lockfile`

For an imported repository that intentionally has no lockfile yet, use an explicit temporary override instead of duplicating the whole CI workflow:

```yaml
    with:
      package-manager: npm
      install-command: npm install --no-audit --no-fund
```

Introduce a lockfile deliberately later and then remove the override so CI returns to frozen installs.

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

For a stable production alias, a caller may also run the smoke workflow after `main` pushes and on a low-frequency schedule. Keep the URL in the caller repository because deployment targets are project-specific.

## First production pilot — BAEV

`IMONsergey/BAEV-WEBSITE` validates the initial adoption pattern:

- private consumer repository;
- central Node CI via `@v1`;
- explicit lockfile-less npm install override;
- Vite build artifact upload;
- local project-specific regression guard;
- shared production HTTP smoke workflow;
- six-hour production availability schedule.

Use this pattern as the reference for similar Figma Make/Vite repositories, then tighten each project toward a committed lockfile and browser E2E coverage.

## Repository rules

After CI is stable, protect `main` with required status checks and pull requests. Do not make a check required before verifying that every active repository can actually produce that check.
