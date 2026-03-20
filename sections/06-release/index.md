---
title: Release
has_children: false
nav_order: 7
---

# Release

## Artefacts

The project produces one deployed artefact: the **backend service**, packaged and deployed as a Docker container to **Fly.io**. The frontend is a static React/Vite build that is not separately published to any package registry.

No artefacts are released to PyPI, Docker Hub, GitHub Packages, or NPM.

## How They Are Released

The backend is released **automatically** via GitHub Actions. The workflow `.github/workflows/backend-deploy.yml` is triggered when a git tag matching the pattern `backend/v*` is pushed (e.g. `backend/v1.0.0`). Manual dispatch is also supported.

Steps performed by the workflow:
1. Checkout code with full history (`fetch-depth: 0`)
2. Extract version from the git tag
3. Set up Fly.io CLI (`flyctl`)
4. Run `flyctl deploy --remote-only` from the `backend/` directory using the `FLY_API_TOKEN` secret

## Choice of the License

The project uses the **MIT License** for both the code and the artefacts. MIT was chosen because it is permissive, widely recognised, and imposes minimal restrictions on use, modification, and redistribution.

## Choice of the Versioning Schema

The project uses **Semantic Versioning (SemVer)** with a component prefix for the backend: `backend/vMAJOR.MINOR.PATCH`.

- **MAJOR** — breaking changes to the API or behaviour
- **MINOR** — new features, backwards-compatible
- **PATCH** — bug fixes

Since only the backend has a release workflow, it has its own independent versioning pace. The frontend version in `package.json` is currently `0.0.0` and is not published.

**Creating a new release (automated):**
1. Merge changes into `main`
2. Create and push a tag: `git tag backend/v1.2.0 && git push origin backend/v1.2.0`
3. The deploy workflow triggers automatically and deploys to Fly.io

**Creating a new release (manual, local):**

If you have the Fly.io CLI installed and are authenticated (`flyctl auth login`), you can deploy directly from your machine:

```bash
cd backend
flyctl deploy
```

The difference from the automated workflow is that the Docker image is built locally before being uploaded to Fly.io, whereas the automated workflow uses `--remote-only` and delegates the build to Fly.io's servers.

