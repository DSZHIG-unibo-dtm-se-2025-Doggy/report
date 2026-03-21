---
title: Developer guide
has_children: false
nav_order: 11
---

# Developer Guide

This section explains how new contributors can start working on Doggy – Dog Breed Identifier. It includes collaboration procedure, conventions, and local setup.

---

## Communication and issue reporting

The project is managed on GitHub. Contributors can:

- open issues for bugs, enhancements, or questions;
- comment on issues and pull requests;
- submit pull requests from feature branches.

Use GitHub issues for planning and backlog; no additional tools are required.

---

## Coding conventions and style

- Backend is Python 3.12, style follows PEP 8.
- Frontend is TypeScript + React; follow common React best practices.
- File names use snake_case for Python. Frontend currently uses mostly PascalCase for React pages/components; keep existing style consistent within each folder.
- Commit messages use Conventional Commits format:
  - `feat(api): add /api/dog-from-photo endpoint`
  - `fix(core): handle non-dog upload with error message`
  - `chore(deps): update transformers package`

---

## Development environment setup

1. Clone repo:
   ```bash
   git clone https://github.com/<your-org>/artifact.git
   cd artifact
   ```

2. Backend virtualenv:
   ```bash
   cd backend
   python -m venv venv
   venv\Scripts\activate    # Windows
   # source venv/bin/activate  # Linux/Mac
   pip install -r requirements.txt -r requirements-dev.txt
   ```

3. Frontend dependencies:
   ```bash
   cd ../web
   npm install
   ```

4. Run tests:
   - Backend: from project root — `PYTHONPATH=. python -m pytest --cov=backend --cov-report=term-missing backend/tests`
   - Frontend: `cd ../web && npm run test:ci`

5. Start app:
   - Backend: from project root — `python -m uvicorn backend.main:app --reload --port 8000`
   - Frontend: `cd ../web && npm run dev -- --host 0.0.0.0 --port 5173`

---

## Development workflow

1. Create a feature branch from `main`:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feat/your-feature
   ```
2. Implement changes, run tests locally.
3. Stage and commit using Conventional Commits:
   ```bash
   git add .
   git commit -m "feat(api): add /api/dog-advice endpoint"
   ```
4. Push and open PR:
   ```bash
   git push origin feat/your-feature
   ```
5. Open a pull request targeting `main`, request review, fix requested changes.
6. After merge, tag release if needed and run deploy pipeline.

---

## Development tools

- Recommended IDE: Visual Studio Code (VS Code) with:
  - Python extension
  - ESLint and Prettier for frontend
  - Pylance for Python intellisense
- Linter & formatters:
  - Backend: `ruff` (check with `python -m ruff check backend`)
  - Frontend: `eslint`/`prettier` (via `npm run lint`)

- CI/CD: GitHub Actions workflows in `.github/workflows` (`backend-ci.yml`, `web-ci.yml`, `backend-deploy.yml`). Ensure all tests pass before PR merge.

- Browser tools: use Chrome/Edge DevTools and React DevTools for front-end debugging.
- Model debugging: inspect backend logs for `transformers` model load paths and gather latency from the `/api/dog-from-photo` request timings.


