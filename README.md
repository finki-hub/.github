# finki-hub/.github

Organization wide defaults and reusable GitHub Actions for finki-hub.

## What’s Inside

- [profile/README.md](profile/README.md): Organization profile shown on the org page.
- Reusable workflows in [.github/workflows/](.github/workflows/): ESLint, Ruff, mypy, TSC, Vitest, Docker, Cloudflare Pages, Cloudflare Workers, Semantic Release, and Dependabot.
- Community health files in [.github/](.github/): code of conduct, security policy, PR template, and issue templates.
- Repo defaults: [.editorconfig](.editorconfig) and [.sonarcloud.properties](.sonarcloud.properties).

## Reuse a Workflow

Create a workflow in another repo that calls one from here:

```yaml
name: Lint
on: [push, pull_request]
jobs:
  lint:
    uses: finki-hub/.github/.github/workflows/eslint.yaml@main
    secrets: inherit
```
