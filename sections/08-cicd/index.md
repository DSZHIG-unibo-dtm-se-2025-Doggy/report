---
title: CI/CD
has_children: false
nav_order: 9
---

# CI/CD

The project uses **GitHub Actions** for CI/CD with three workflows.

## Continuous Integration

**Backend CI** (`.github/workflows/backend-ci.yml`) triggers on push and pull requests to `main` and `dev` branches when backend files change. It runs two jobs:

- `lint-and-type-check`: runs `ruff` (linting and formatting) and `mypy` (type checking)
- `test`: runs `pytest` with coverage across Python 3.10, 3.11, and 3.12 on Ubuntu, macOS, and Windows

**Web CI** (`.github/workflows/web-ci.yml`) triggers on pull requests to `dev`. It runs:

- `lint-and-type-check`: runs ESLint
- `test`: runs Vitest in CI mode

Automation ensures that every change is validated before merging, catching type errors, style violations, and broken tests early.

## Continuous Deployment

**Backend Deploy** (`.github/workflows/backend-deploy.yml`) triggers automatically when a git tag matching `backend/v*` is pushed, or manually via workflow dispatch. It deploys the backend to Fly.io using `flyctl deploy --remote-only`.

## Secrets and Environment Variables

| Name | Where | Purpose |
|------|-------|---------|
| `FLY_API_TOKEN` | GitHub Secret | Authenticates `flyctl` during deployment |
| `HF_TOKEN` | Fly.io Secret | HuggingFace API token for advice generation (set via `flyctl secrets set`) |
| `HF_TOKEN=dummy` | CI env var | Allows backend tests to run without a real token |
