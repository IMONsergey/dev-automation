# Security Policy

## Scope

This repository defines reusable automation consumed by other repositories. A workflow change can therefore affect multiple projects.

## Rules

- Never commit credentials or project secrets.
- Keep `GITHUB_TOKEN` permissions minimal.
- Treat pull-request content as untrusted.
- Avoid executing pull-request-controlled strings in privileged contexts.
- Do not use `pull_request_target` for workflows that execute untrusted repository code.
- Prefer official Actions and stable major versions; pin third-party Actions more strictly when introduced.
- Review changes to reusable workflow inputs, permissions, secret inheritance, deployment, and release behavior as security-sensitive.

## Reporting

Report security-sensitive problems privately to the repository owner rather than opening a public exploit-ready issue.
