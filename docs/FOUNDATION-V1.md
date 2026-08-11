# Foundation v1

Initial compatibility contract for `IMONsergey/dev-automation`.

## Included

- Reusable Node CI for npm, pnpm, yarn, and bun.
- Reusable Playwright browser testing with retained reports and test results.
- Reusable CodeQL scanning.
- Reusable HTTP deployment smoke checks.
- Foundation self-validation with actionlint.
- Dependabot maintenance for GitHub Actions dependencies.
- Agent operating rules and security policy.

## Compatibility

The stable `v1` branch may receive backward-compatible improvements. Breaking changes to workflow inputs, outputs, or behavior require a new major compatibility branch.

## Next layers

- Lighthouse/performance gates.
- Vercel preview orchestration and health verification.
- Standard release/build artifact workflows.
- Sentry and PostHog feedback loops.
- Automated adoption across active `IMONsergey/*` repositories.
