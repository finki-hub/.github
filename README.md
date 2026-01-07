# finki-hub/.github

Org-wide defaults and reusable GitHub Actions for finki-hub.

## What’s Inside

- [profile/README.md](profile/README.md): Organization profile shown on the org page.
- [.github/workflows/eslint.yaml](.github/workflows/eslint.yaml): Reusable ESLint workflow for other repos.
- Optional community health files (issue/PR templates, etc.) if added under `.github/`.

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
