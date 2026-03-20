---
title: Requirements
has_children: false
nav_order: 3
---

# Requirements

## User Stories

Main persona: a user who wants to identify dog breeds fast from a camera/photo, in a web browser (desktop or mobile).  
The app must manage image upload, breed inference, and text advice generation.

- **US1**: As a user, I want to upload a dog photo so I can get a breed prediction.
- **US2**: As a user, I want to see a detected breed and AI recommendations for care.
- **US3**: As a user, I want to retry with another photo if the result is wrong.
- **US4**: As a user, I want to see a clear error when the uploaded image is not a dog.
- **US5**: As an admin, I want the backend to support a breed catalog update API.
- **US6**: As a developer, I want health checks and a stable API.
- **US7**: As a developer, I want a simple dev workflow (`pytest`, `vitest`, `ruff`, Docker/Fly`).

---

## Requirements Analysis

The system uses a FastAPI backend and React frontend with ML inference.

### Functional Requirements (FR)

| ID | Description | Acceptance Criteria |
|----|-------------|---------------------|
| <a id="fr1"></a>**FR1** | `POST /api/dog-from-photo` accepts file and returns result. | JSON contains `breed`, `raw_predictions`, `advice`; or `error` for non-dog input. |
| <a id="fr2"></a>**FR2** | `POST /api/dog-advice?breed=<name>` returns breed advice. | JSON contains `advice`. |
| <a id="fr3"></a>**FR3** | Non-dog input gets a clear error. | JSON contains `error`: "Sorry, this is not a dog. Please try again". |
| <a id="fr4"></a>**FR4** | Backend uses DogRecognition and LLM modules. | `backend/Features/DogRecognition/dog_recognition.py` and `backend/Features/LLM/llm_engine.py` are active. |
| <a id="fr5"></a>**FR5** | Health endpoint available. | `GET /` returns `{"status":"ok","message":"backend is running"}`. |
| <a id="fr6"></a>**FR6** | User can retry upload flows. | UI allows multiple upload attempts. |

### Non-Functional Requirements (NFR)

| ID | Description | Acceptance Criteria |
|----|-------------|---------------------|
| **NFR1** | Fast response times. | 95% of requests < 2s in benchmark tests. |
| **NFR2** | High availability. | 99.5% uptime target for deployed service. |
| **NFR3** | Security: CORS/CSP/XSS matters. | CORS middleware on, OWASP critical issues none. |
| **NFR4** | Responsive UI. | Works on desktop/mobile; accessible (WCAG AA). |
| **NFR5** | No user data persist. | No personal data stored; files are temporary.

### Implementation Requirements (IR)

| ID | Description | Justification | Acceptance Criteria |
|----|-------------|--------------|---------------------|
| <a id="ir1"></a>**IR1** | Python 3 + FastAPI backend. | Existing codebase language and framework. | `python -m uvicorn backend.main:app` runs. |
| <a id="ir2"></a>**IR2** | React + Vite + TypeScript frontend. | Existing web project stack. | `npm run build` passes. |
| <a id="ir3"></a>**IR3** | Test tooling `pytest`, `vitest`, `ruff`. | CI standard. | all tests pass. |
| <a id="ir4"></a>**IR4** | Docker and Fly deployment config exist. | Provided project files. | container builds and starts. |
| <a id="ir5"></a>**IR5** | Env var configuration. | security in deployments. | settings driven via env. |

---

## Glossary

- **Breed**: predicted dog type from model.
- **raw_predictions**: top-3 model outputs (`label`, `score`).
- **advice**: text from `DogLLMEngine.generate_advice`.
- **inference**: model pipeline running on image.
- **health check**: `GET /` endpoint.
- **API**: FastAPI endpoints in `backend/main.py`.
- **rate-limit**: deployment request throttling.

---

## Acceptance Criteria

- [FR1](#fr1): `/api/dog-from-photo` returns breed/advice (or error response). 
- [FR2](#fr2): `/api/dog-advice` returns advice.
- [FR3](#fr3): non-dog returns `error` message.
- NFR1: 95% < 2s.
- [IR2](#ir2): `npm run build` passes.
- [IR3](#ir3): `pytest` + `vitest` + `ruff` pass.
