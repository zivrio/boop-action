# Contributing to boop-action

Thank you for your interest in contributing!

## Reporting issues

Open an issue and include:
- The workflow YAML that triggered the problem
- The full action log output
- Your runner OS and the boop CLI version (printed at the start of the action)

## Suggesting changes

Open an issue before submitting a pull request for any significant change.
For small fixes (typos, docs), a PR directly is fine.

## Pull requests

1. Fork the repository
2. Create a branch: `git checkout -b fix/my-fix`
3. Make your changes to `action.yml` and `README.md`
4. Test locally by referencing your fork in a workflow:
   ```yaml
   uses: your-username/boop-action@your-branch
   ```
5. Open a pull request against `main`

## Versioning

Releases follow [semantic versioning](https://semver.org):
- **Patch** (`v1.0.x`) — bug fixes, no new inputs
- **Minor** (`v1.x.0`) — new optional inputs, new outputs
- **Major** (`vX.0.0`) — removed/renamed inputs, breaking behaviour changes

The `v1` tag is updated to point to the latest `v1.x.x` on every release.

## License

By contributing you agree that your changes will be licensed under the [MIT License](LICENSE).
