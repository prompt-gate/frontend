# Environment Variables

This page documents every environment variable used by the frontend project,
the Docker image, and the GitHub Actions workflows.

## Application Runtime

| Variable | Required | Default | Used by | Description |
| --- | --- | --- | --- | --- |
| `NUXT_PUBLIC_API_BASE_URL` | Yes for authenticated deployments | Empty string | Nuxt runtime config | Public backend API base URL used for authentication and API calls. The value is normalized by removing one trailing slash. |

### `NUXT_PUBLIC_API_BASE_URL`

Set this variable to the externally reachable Prompt Gate backend URL:

```sh
NUXT_PUBLIC_API_BASE_URL=https://api.example.com
```

The frontend uses it for:

- loading the current session from `/auth/session`;
- redirecting users to `/auth/login`;
- redirecting users through `/auth/logout`;
- sending authenticated API requests with browser credentials.

If the value is missing or empty, the application marks backend authentication
as unavailable and shows the configured error state.

## Docker Runtime

These variables are defined in the `Dockerfile` and can be overridden by the
deployment platform when needed.

| Variable | Required | Default | Used by | Description |
| --- | --- | --- | --- | --- |
| `NODE_ENV` | No | `production` | Node.js and dependencies | Enables production behavior in the runtime container. |
| `NITRO_HOST` | No | `0.0.0.0` | Nuxt Nitro server | Makes the server listen on every container network interface. Keep this value in containers. |
| `NITRO_PORT` | No | `3000` | Nuxt Nitro server | Port used by the Nitro server inside the container. The Docker image exposes `3000`. |

Example:

```sh
docker run --rm \
  -p 3000:3000 \
  -e NUXT_PUBLIC_API_BASE_URL=https://api.example.com \
  ghcr.io/prompt-gate/frontend:v0.1.0
```

The container runs as `appuser` with UID/GID `1000`. This is not configured
through an environment variable.

## GitHub Actions

The workflows use a mix of explicit workflow variables and automatic GitHub
variables.

### CI Workflow

The CI workflow does not require custom environment variables. It runs on branch
pushes and pull requests, builds the Nuxt app, builds the Docker image, and
checks that the container user is `appuser` with UID `1000`.

### Release Workflow

| Variable | Source | Description |
| --- | --- | --- |
| `REGISTRY` | Workflow env | Container registry host. Current value: `ghcr.io`. |
| `IMAGE_NAME` | Workflow env | Image path inside the registry. Current value: `prompt-gate/frontend`. |
| `GITHUB_TOKEN` | GitHub secret | Automatic token used to push the GHCR image and create the GitHub Release. |
| `GITHUB_REF_NAME` | GitHub Actions | Tag name that triggered the release, for example `v0.1.0`. |
| `GITHUB_SHA` | GitHub Actions | Commit SHA used to create the `sha-<short-sha>` Docker tag. |
| `GITHUB_REPOSITORY` | GitHub Actions | Repository slug passed to the GitHub CLI when creating the release. |
| `GITHUB_OUTPUT` | GitHub Actions | File path used by a step to expose outputs to later steps. |
| `TAG_NAME` | Step env | Local step variable passed to `gh release create`. |
| `VERSION` | Shell variable in docs/workflow | Semver value without the leading `v`, derived from the Git tag or set locally before creating a tag. |
| `SHORT_SHA` | Shell variable in workflow | Seven-character SHA derived from `GITHUB_SHA` and used for the `sha-<short-sha>` Docker tag. |

For a release tag `v0.1.0`, the workflow publishes:

```text
ghcr.io/prompt-gate/frontend:v0.1.0
ghcr.io/prompt-gate/frontend:0.1.0
ghcr.io/prompt-gate/frontend:latest
ghcr.io/prompt-gate/frontend:sha-<short-sha>
```

## Local `.env` Files

Local `.env` files are ignored by Git. Use them for developer-specific values
only:

```sh
NUXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

Do not commit secrets or environment-specific credentials. This frontend does
not currently require secret environment variables.
