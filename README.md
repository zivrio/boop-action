# boop-action

[![GitHub release](https://img.shields.io/github/v/release/zivrio/boop-action?label=latest)](https://github.com/zivrio/boop-action/releases)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Boop%20Code%20Quality-blue?logo=github)](https://github.com/marketplace/actions/boop-code-quality)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Run [Boop](https://github.com/zivrio/boop-cli) code quality checks in CI. Scans changed files on every push or pull request and fails the job if quality gate checks don't pass.

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
```

---

## Quick start

### 1. Install the CLI and init your project

```sh
# macOS / Linux
curl -fsSL https://github.com/zivrio/boop-cli-releases/releases/latest/download/boop_linux_amd64 \
  -o /usr/local/bin/boop && chmod +x /usr/local/bin/boop

boop register          # create an account
boop login             # authenticate
boop init my-project   # creates .boop/config.json — commit this file
boop scan              # index your code for the first time
```

### 2. Add the secret

Generate a project-scoped key and add it to your repository:

```sh
boop project keys
```

Go to **Settings → Secrets and variables → Actions** and add `BOOP_PROJECT_KEY`.

### 3. Add the workflow

Create `.github/workflows/boop.yml`:

```yaml
name: Boop Code Quality

on:
  push:
    branches: [main]
  pull_request:

jobs:
  boop:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: zivrio/boop-action@v1
        with:
          project-key: ${{ secrets.BOOP_PROJECT_KEY }}
```

---

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `project-key` | recommended | — | Project-scoped API key. Preferred over `api-key` in CI — limits access to this project only. |
| `api-key` | fallback | — | Full account API key. Use `project-key` instead when possible. |
| `project-id` | no | from `.boop/config.json` | Project ID override. Only needed if you haven't committed `.boop/config.json`. |
| `category` | no | `all` | Check category: `security`, `complexity`, `quality`, or `all`. |
| `scan` | no | `true` | Set to `'false'` to skip scanning and only re-run checks against the existing index. |
| `fail-on` | no | `errors` | `errors` — fail on error-severity issues. `warnings` — also fail on warnings. `never` — advisory mode, always exits 0. |
| `version` | no | `latest` | Pin the CLI version, e.g. `'0.1.2'`. Defaults to latest release. |

## Outputs

| Output | Description |
|--------|-------------|
| `issues` | Total number of issues found across all checks. |
| `result` | Overall result: `passed` or `failed`. |

---

## Examples

### Security checks only

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    category: security
```

### Fail on warnings (strict mode)

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    fail-on: warnings
```

### Advisory only — never block the PR

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    fail-on: never
```

### Skip scan, re-check existing index (faster)

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    scan: 'false'
```

### Use outputs in subsequent steps

```yaml
- uses: zivrio/boop-action@v1
  id: boop
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    fail-on: never

- name: Comment on PR
  if: github.event_name == 'pull_request'
  run: |
    echo "Boop found ${{ steps.boop.outputs.issues }} issue(s) (${{ steps.boop.outputs.result }})"
```

### Pin to a specific CLI version

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    version: '0.1.2'
```

### Without committing `.boop/config.json`

```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    project-id: ${{ secrets.BOOP_PROJECT_ID }}
```

---

## Permissions

This action does not require any additional GitHub token permissions beyond the default `GITHUB_TOKEN`. It uses `BOOP_PROJECT_KEY` (or `BOOP_API_KEY`) to authenticate with the Boop API.

## Supported runners

| Runner | Supported |
|--------|-----------|
| `ubuntu-*` | ✅ |
| `macos-*` | ✅ |
| `windows-*` | ❌ (planned) |

## Versioning

This action follows [semantic versioning](https://semver.org). The `v1` tag always points to the latest `v1.x.x` release — using `@v1` means you get bug fixes and new features automatically without breaking changes.

To pin to an exact version: `uses: zivrio/boop-action@v1.0.0`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
