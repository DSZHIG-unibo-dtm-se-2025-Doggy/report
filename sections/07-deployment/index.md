---
title: Deployment
has_children: false
nav_order: 8
---

# Deployment

This section explains what operations are needed to make the software work on the users' machine(s)

## User Installation

The user does not need to install anything. Doggy is a web application accessible through any modern browser. No plugins, desktop clients, or configuration are required.

## Server-side Installation

The backend runs as a Docker container on **Fly.io**. To deploy it:

1. Install the Fly.io CLI:
```bash
brew install flyctl
```

2. Authenticate:
```bash
flyctl auth login
```

3. Set the required environment variable — the HuggingFace API token used for advice generation:
```bash
flyctl secrets set HF_TOKEN=<your_token> --app doggy-black-darkness-694
```

4. Deploy from the `backend/` directory:
```bash
cd backend
flyctl deploy
```

The deployment configuration is defined in `backend/fly.toml`: the app runs on port 8080, in the Amsterdam region, with 1 CPU and 2 GB RAM.

No additional software is required on the server — there is no database, no message broker, and no persistent storage. The application is fully stateless.