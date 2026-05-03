# Release Creation

Releases are created by GitHub Actions when a semver tag in the `vX.Y.Z` format
is pushed to the repository.

## Requirements

- Write access to the `prompt-gate/frontend` repository.
- GitHub Packages enabled for the repository.
- The workflow `GITHUB_TOKEN` allowed to publish packages and releases.
- A local build verified before pushing a tag.

See [Environment variables](environment.md) for the release and workflow
variable reference.

## Pre-release Verification

From `main`:

```sh
git checkout main
git pull --ff-only origin main
npm ci
npm run typecheck
npm test -- --run
npm run build
docker build -t prompt-gate-frontend:test .
docker run --rm --entrypoint id prompt-gate-frontend:test
```

The `id` command must print `uid=1000(appuser)`.

## Create a Tag

Choose a semver version without the `v` prefix, then create the tag with the
`v` prefix.

```sh
export VERSION=0.1.0
git tag -a "v${VERSION}" -m "Release v${VERSION}"
git push origin "v${VERSION}"
```

Pushing the tag triggers the `Release` workflow.

## Pipeline Behavior

The pipeline runs:

- dependency installation with `npm ci`;
- TypeScript verification with `npm run typecheck`;
- tests with `npm test -- --run`;
- Nuxt build with `npm run build`;
- Docker image build and push;
- GitHub Release creation with generated release notes.

## Published Docker Tags

For a Git tag `v0.1.0`, the pipeline publishes:

```text
ghcr.io/prompt-gate/frontend:v0.1.0
ghcr.io/prompt-gate/frontend:0.1.0
ghcr.io/prompt-gate/frontend:latest
ghcr.io/prompt-gate/frontend:sha-<short-sha>
```

## Post-release Verification

After the workflow completes:

```sh
docker pull ghcr.io/prompt-gate/frontend:v0.1.0
docker run --rm --entrypoint id ghcr.io/prompt-gate/frontend:v0.1.0
```

Also verify that the GitHub Release exists in the repository Releases tab.

## Rollback

To roll back to a previous version, redeploy the previous stable Docker tag:

```sh
docker pull ghcr.io/prompt-gate/frontend:v0.0.9
```

Then update the deployment platform to use that tag. Avoid `latest` in
production environments so rollbacks stay predictable.
