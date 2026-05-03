# Prompt Gate Frontend

Prompt Gate Nuxt frontend. The application uses Nuxt 4, Vue 3, Vuetify, Pinia,
and Vitest.

## Requirements

- Node.js 24
- npm
- Docker, only when building or testing the production image

## Installation

```sh
npm ci
```

## Development

```sh
npm run dev
```

By default, the application expects the backend URL in the following public Nuxt
variable:

```sh
NUXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

## Quality and Build

```sh
npm run typecheck
npm test -- --run
npm run build
```

The `npm run lint` script runs ESLint with automatic fixes enabled.

## Documentation

- [Docker and GHCR deployment](docs/deployment.md)
- [Environment variables](docs/environment.md)
- [Release creation](docs/release.md)

## Release

Official releases are created from a semver tag in the `vX.Y.Z` format. Pushing
the tag triggers the GitHub Actions pipeline, publishes the
`ghcr.io/prompt-gate/frontend` image, and creates the GitHub Release.
