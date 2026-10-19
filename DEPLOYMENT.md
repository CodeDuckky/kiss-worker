# Automatic deployment

Repository: https://github.com/CodeDuckky/kiss-worker

Production Worker: `kiss-worker`. The production branch is `master`.

Cloudflare Workers Builds is the deployment owner. Pushes and merges to
`master` first run `npm run check` and then `npx wrangler deploy`.
The repository root is `/`, Node.js is pinned to 22 in `.node-version`, and
non-production branch builds are disabled.

GitHub Actions runs the same checks on pushes and pull requests, without a
second deployment job. Dependabot checks npm dependencies and GitHub Actions
weekly and opens PRs. Updates require review and a merge; no auto-merge is set.

KISS keeps the existing `KV` and `SYNC_OBJECT` bindings. The existing `AUTH_VALUE` runtime secret remains in Cloudflare; do not put its value in Git or build variables. Tests use isolated local storage and a test-only secret.

## Local checks

```sh
npm ci
npm run check
```

## Rollback

Restore a prior deployment in the existing Worker's Cloudflare Deployments
page, then revert the problem change on `master` to align the source.
Pre-automation deployment version: `22584b84-c54c-47c3-8444-4b1fbc6d412c`.

This change configures CI and deployment; it does not change application logic.
