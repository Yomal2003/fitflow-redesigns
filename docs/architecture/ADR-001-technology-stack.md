# ADR-001: FitFlow Technology Stack and High-Level Architecture

**Status:** Accepted
**Date:** 2026 (Semester 2), IT3060 Lab Exercise 05
**Deciders:** Yomal Basnayaka (FitFlow redesign — coursework project)

## Context

The FitFlow redesign requires a technology stack capable of supporting three new headline
features — AI-personalized workout plans, a private social community, and camera-based
nutrition tracking — across iOS, Android, and web, for a mid-sized team with a limited budget and
a need to ship quickly to reverse a churn/rating decline. The stack must also handle
health-adjacent user data (activity, nutrition) responsibly under GDPR/CCPA-style obligations.

## Decision

Adopt the following stack, selected via the weighted decision matrix in
[`docs/comparisons/weighted-decision-matrix.md`](../comparisons/weighted-decision-matrix.md):

- **Frontend:** React Native (Expo) + React Native Web — single codebase for iOS, Android, web
- **Backend:** NestJS (Node.js/TypeScript) as the core API and gateway
- **AI microservice:** FastAPI (Python) + TensorFlow Lite, deployed as an isolated service
- **Primary database:** PostgreSQL — system of record for users, plans, and nutrition logs
- **Real-time store:** Firebase Firestore / Realtime DB — social feed, presence, live nudges
- **Authentication:** Firebase Auth
- **Cache:** Redis

## Alternatives considered

- **Flutter-based stack (Stack B):** marginally better raw performance, but Dart introduces a new
  language with no existing team experience, and its web target is less mature for a
  content-heavy, accessibility-sensitive app. Scored 4.35/5.0 vs Stack A's 4.55/5.0.
- **Go + DynamoDB + Cognito stack (Stack C):** highest raw performance ceiling, but introduces a
  third backend language (on top of TypeScript and Python for the AI service), the weakest
  ecosystem fit for the AI/social feature set, and the least team familiarity. Scored 3.80/5.0.
- **Single-database approach (PostgreSQL only, or Firestore only):** rejected in favor of polyglot
  persistence — PostgreSQL alone would require hand-building real-time sync for the social feed;
  Firestore alone would weaken relational integrity and auditability for nutrition/workout data.

## Consequences

**Positive:**
- Shared language (TypeScript) across frontend and backend reduces context-switching for a
  small team and speeds up onboarding/maintenance.
- Firebase (Auth + Firestore) minimizes the number of third-party vendors the team must
  integrate and operate, appropriate for a startup-scale team.
- Isolating the AI microservice means the ML stack's dependencies and scaling needs never
  destabilize the core API, and the ML/data science team can iterate independently.
- All chosen components have low or no licensing cost at the team's current scale.

**Negative / trade-offs accepted:**
- Two backend languages (TypeScript + Python) rather than one, accepted because Python is
  effectively non-negotiable for serious ML work.
- Firebase introduces a degree of vendor lock-in for real-time/auth features; judged acceptable
  given the integration-speed and cost benefits at this stage.
- React Native's performance ceiling is slightly below Flutter's or a fully native approach; judged
  acceptable because FitFlow's UI demands (dashboards, forms, a social feed) do not push against
  that ceiling the way a graphics-intensive app would.

## Re-evaluation triggers

This decision should be revisited if any of the following occur:
- FitFlow needs formal HIPAA certification (would prompt re-evaluating Auth0/AWS Cognito and a
  more formally certified database/hosting setup over Firebase).
- The app's UI/animation demands grow substantially more graphics-intensive (would prompt
  re-evaluating Flutter or native development).
- Team headcount or AWS-specific expertise grows significantly (would reopen the Go/DynamoDB
  option evaluated as Stack C).
