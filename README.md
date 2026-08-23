# finki-hub/.github

Organization wide defaults and reusable GitHub Actions for finki-hub.

## What’s Inside

- [profile/README.md](profile/README.md): Organization profile shown on the org page.
- Reusable workflows in [.github/workflows/](.github/workflows/): documentation checks, ESLint, Ruff, mypy, TSC, Vitest, Pytest, Pytest with PostgreSQL, Docker, Cloudflare Pages, Cloudflare Workers, Semantic Release, and Dependabot.
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
```

Use `@main` to follow shared workflow updates immediately. Use a release tag or
commit SHA when a repository needs a reviewed, stable revision.

## Workflow Contracts

| Workflow | Purpose | Primary inputs | Caller requirements |
| --- | --- | --- | --- |
| `docs-check.yaml` | Lint and build documentation | Node version, working directory, lint script, build script | `package-lock.json` and the configured npm scripts |
| `eslint.yaml` | Run ESLint | Node version, working directory, npm script | `package-lock.json` and an ESLint npm script |
| `tsc.yaml` | Run TypeScript checks | Node version, working directory, optional npm script | `package-lock.json`; TypeScript or the configured npm script |
| `vitest.yaml` | Run Vitest, optionally as a matrix | Node versions, working directory, test arguments, matrix controls | `package-lock.json` and a `test` npm script; optional `CAS_USERNAME` and `CAS_PASSWORD` secrets |
| `playwright.yaml` | Run browser tests in the Playwright container | Node version, working directory, container, commands, environment variables | `package-lock.json` and the configured test commands |
| `pytest.yaml` | Run Pytest with uv | Python version, working directory, sync arguments, Pytest arguments | A current committed `uv.lock` by default |
| `pytest-postgres.yaml` | Run Pytest with PostgreSQL and pgvector | Python version, working directory, sync arguments, Pytest arguments, PostgreSQL image | A current committed `uv.lock`; tests must use `TEST_DATABASE_URL` |
| `ruff.yaml` | Run Ruff lint and format checks | Working directory | A current committed `uv.lock` with Ruff in the development dependencies |
| `mypy.yaml` | Run MyPy | Working directory | A current committed `uv.lock` with MyPy in the development dependencies |
| `docker.yaml` | Build and publish multi-platform images to GHCR | Image name, platforms, Dockerfile, context, cache, build arguments | `contents: read` and `packages: write`; build arguments must not contain secrets |
| `cloudflare-pages.yaml` | Build and deploy Cloudflare Pages | Node version, working directory, project name, output directory | `package-lock.json` plus `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` |
| `cloudflare-workers.yaml` | Deploy Cloudflare Workers | Node version, worker directory, install directory | `package-lock.json` plus `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` |
| `semantic-release.yaml` | Verify and publish semantic releases | None | `semantic-release` installed from `package-lock.json`; `lint`, `test`, and `build` scripts; caller grants contents, issues, pull requests, and ID token write permissions; npm trusted publisher targets the caller workflow filename |
| `dependabot.yaml` | Approve and auto-merge non-major Dependabot updates | None | `contents: write` and `pull-requests: write`; required CI checks configured in repository rules |

## Caller Guidelines

- Pass only the secrets declared by a workflow. Avoid `secrets: inherit` unless
  every caller secret is intentionally available to the reusable workflow.
- Keep workflow inputs in version-controlled caller files. Do not populate
  command, path, image, or environment inputs from untrusted event data.
- Keep `package-lock.json` or `uv.lock` current whenever the corresponding
  workflow installs dependencies in locked mode.
- Grant write permissions only to Docker publishing, releases, and Dependabot
  auto-merge callers. Lint and test workflows require only `contents: read`.
- Configure required status checks before enabling Dependabot auto-merge.
