# boop-action

Run [Boop](https://boop-api-production.up.railway.app) code quality checks in GitHub Actions.
Scans your code on every push or pull request and fails the job if quality gate checks don't pass.

## Usage

```yaml
# .github/workflows/boop.yml
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

## Setup

1. **Create an account** — `boop register` or sign up on the website
2. **Init your project** — run `boop init my-project` in your repo root; commit the generated `.boop/config.json`
3. **Index your code** — run `boop scan` once locally
4. **Add the secret** — go to your repo's **Settings → Secrets → Actions** and add `BOOP_PROJECT_KEY` with the project key from `boop project keys`

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `project-key` | recommended | — | Project-scoped API key (preferred for CI) |
| `api-key` | fallback | — | Full account API key (use if no project key yet) |
| `project-id` | no | from `.boop/config.json` | Project ID override |
| `category` | no | `all` | Check category: `security`, `complexity`, `quality`, or `all` |
| `scan` | no | `true` | Set to `false` to skip scan and only run checks |
| `fail-on` | no | `errors` | When to fail: `errors`, `warnings`, or `never` |

## Examples

**Security checks only on PRs:**
```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    category: security
```

**Check without re-scanning (faster, uses existing index):**
```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    scan: 'false'
```

**Fail on warnings too:**
```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    fail-on: warnings
```

**Without a committed `.boop/config.json`:**
```yaml
- uses: zivrio/boop-action@v1
  with:
    project-key: ${{ secrets.BOOP_PROJECT_KEY }}
    project-id: ${{ secrets.BOOP_PROJECT_ID }}
```
