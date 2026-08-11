# AGENTS.md

## Operating model

GitHub is the source of truth for this repository.

Agents should act autonomously when the requested operation can be completed with connected tools. Do not ask the user to copy code, create files, or run Git commands manually when the available GitHub tooling can do it.

## Before changes

1. Read the current repository state from GitHub.
2. Inspect relevant workflows, documentation, open PRs, and issues.
3. Preserve backward compatibility for callers using the current major compatibility branch unless a breaking change is explicitly intended.
4. Prefer official GitHub-maintained Actions where an appropriate official Action exists.

## Validation

For workflow changes:

- Validate YAML syntax.
- Check `workflow_call` inputs and secret contracts.
- Keep permissions minimal.
- Avoid untrusted interpolation into privileged shell contexts.
- Ensure artifacts and logs do not expose secrets.
- Prefer immutable refs or stable major refs for third-party Actions.
- Test changes on a feature branch/PR before advancing a stable compatibility branch.

## Release discipline

- `main`: active development.
- `v1`: stable compatibility branch.
- Breaking changes require a new major compatibility branch.
- Do not move a stable compatibility branch until validation is green.

## Automation principles

- Reusable workflows should be composable and project-agnostic.
- Project-specific secrets stay in caller repositories/environments.
- Fail loudly on real build/test errors.
- Skip only genuinely absent optional scripts.
- Persist diagnostic artifacts when a browser or build job fails.
- Keep workflows readable enough to audit without generated indirection.
