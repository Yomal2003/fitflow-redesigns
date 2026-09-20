# FitFlow Redesign

Technology stack research, architecture design, and supporting documentation for the FitFlow
fitness app redesign — produced for **IT3060: Human Computer Interaction (Lab Exercise 05)**,
SLIIT BSc (Hons) in Information Technology, Semester 2, 2026.

FitFlow is a fitness-tracking app redesign centered on three new capabilities: AI-personalized
workout plans, a private social community, and camera-based nutrition tracking. This repository
captures the technology decisions and system design that support that redesign.

## Repository structure

```
fitflow-redesign/
├── frontend/          React Native (Expo) + React Native Web client
├── backend/           NestJS core API
├── ai-service/        FastAPI + TensorFlow Lite personalization microservice
├── docs/
│   ├── comparisons/   Frontend / backend / database / auth comparison tables + weighted decision matrix
│   └── architecture/  High-level architecture diagram + Architecture Decision Record (ADR)
├── .gitignore
└── README.md
```

## Recommended technology stack

| Layer | Choice | Why (short version) |
|---|---|---|
| Frontend | **React Native (Expo) + React Native Web** | Single codebase for iOS, Android, and web; strong ecosystem; matches the team's existing skill set from the original case study |
| Backend | **NestJS (Node.js, TypeScript)** | Structured, testable, first-class WebSocket support for real-time features |
| AI microservice | **FastAPI (Python) + TensorFlow Lite** | Best-in-class ML ecosystem; on-device inference for privacy, cloud fallback for heavier models |
| Primary database | **PostgreSQL** | Relational integrity for user, workout, and nutrition data; mature tooling for health-adjacent data |
| Real-time store | **Firebase Firestore / Realtime DB** | Out-of-the-box real-time sync for the social feed and presence, without hand-rolling WebSocket infra |
| Authentication | **Firebase Auth** | Fast integration, generous free tier, social login support, pairs naturally with the realtime store |
| Cache | **Redis** | Session/token caching and hot-read acceleration for the dashboard and AI plan endpoints |

Full comparison tables, scoring, and justification are in [`docs/comparisons/`](docs/comparisons).
The system architecture and rationale are in [`docs/architecture/`](docs/architecture).

## Getting started (placeholder — not yet implemented)

This repository currently holds design and decision documentation only; application code has not
been scaffolded yet. Each of `frontend/`, `backend/`, and `ai-service/` will be initialized in a
follow-up milestone once the architecture in this repo is reviewed and approved.

## License

Coursework project — SLIIT IT3060, Semester 2 2026. Not licensed for external reuse.
