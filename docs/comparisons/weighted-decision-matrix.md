# Weighted Technology Decision Matrix

This matrix consolidates the frontend, backend, database, and authentication comparisons into
three complete candidate stacks, scored against FitFlow's project priorities. Each criterion is
weighted by importance to FitFlow specifically (e.g. AI/ML support and real-time features are
weighted heavily because they are the redesign's headline new capabilities).

## Weights (out of 100, distributed across criteria)

| Criterion | Weight | Rationale |
|---|---|---|
| Development speed | 15% | Startup needs to ship the redesign quickly to reverse churn |
| Code reusability | 10% | Small team, cannot maintain 2-3 separate codebases |
| Performance | 15% | Smooth animations, real-time feed, and camera-based logging all depend on this |
| Ecosystem support | 10% | Reduces risk and speeds up integration of niche features (e.g. camera ML) |
| Security & compliance | 15% | Health-adjacent data (nutrition, activity) and GDPR/CCPA obligations |
| AI/ML support | 15% | AI-personalized workouts and food recognition are core differentiators |
| Real-time capability | 10% | Social feed, live coaching nudges, presence |
| Cost & maintainability | 10% | Startup budget, small mid-sized team |

## Candidate stacks

- **Stack A (Recommended):** React Native (Expo) + React Native Web · NestJS · FastAPI (AI) ·
  PostgreSQL + Firestore · Firebase Auth
- **Stack B:** Flutter · NestJS · FastAPI (AI) · PostgreSQL + Firestore · Firebase Auth
- **Stack C:** React Native (Expo) + React Native Web · Go backend · FastAPI (AI) · DynamoDB ·
  AWS Cognito

## Scored matrix (1 = poor, 5 = excellent; weighted score = raw score × weight)

| Criterion | Weight | Stack A raw | Stack A weighted | Stack B raw | Stack B weighted | Stack C raw | Stack C weighted |
|---|---|---|---|---|---|---|---|
| Development speed | 15% | 5 | 0.75 | 4 | 0.60 | 3 | 0.45 |
| Code reusability | 10% | 5 | 0.50 | 5 | 0.50 | 4 | 0.40 |
| Performance | 15% | 4 | 0.60 | 5 | 0.75 | 5 | 0.75 |
| Ecosystem support | 10% | 5 | 0.50 | 4 | 0.40 | 3 | 0.30 |
| Security & compliance | 15% | 4 | 0.60 | 4 | 0.60 | 4 | 0.60 |
| AI/ML support | 15% | 4 | 0.60 | 4 | 0.60 | 4 | 0.60 |
| Real-time capability | 10% | 5 | 0.50 | 5 | 0.50 | 4 | 0.40 |
| Cost & maintainability | 10% | 5 | 0.50 | 4 | 0.40 | 3 | 0.30 |
| **Total (out of 5.0)** | 100% | | **4.55** | | **4.35** | | **3.80** |

## Result and rationale

**Stack A wins (4.55 / 5.0)** and is the recommended technology stack for the FitFlow redesign.

- It leads on development speed, ecosystem support, and cost/maintainability — the criteria most
  relevant to a startup team trying to move quickly on a limited budget.
- Stack B (Flutter-based) scores marginally higher on raw performance, but the gap is small
  (0.75 vs 0.60 weighted) and does not offset Stack A's advantages in team fit and ecosystem
  maturity, particularly given the team's continuity with React Native from the original project.
- Stack C (Go + DynamoDB + Cognito) scores highest on raw performance and is a reasonable
  choice for a team already deep in the AWS ecosystem, but it introduces a third backend
  language (Go) and the least mature fit with the AI/social feature set relative to the other two,
  pulling its total down to third place.

**Recommended technology stack:** React Native (Expo) + React Native Web for the frontend;
NestJS for the core backend API; FastAPI + TensorFlow Lite for the AI microservice; PostgreSQL
as the primary relational database; Firebase Firestore/Realtime DB for real-time social features;
Firebase Auth for authentication; Redis for caching.
