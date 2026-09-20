# Backend, Database, and Authentication Comparison

## 1. Backend frameworks

| Criterion | Node.js / NestJS | Python / FastAPI | Go |
|---|---|---|---|
| Performance | Good (event loop, non-blocking I/O) | Good (ASGI, async support) | Excellent (compiled, very low latency) |
| Real-time capability | Excellent (native WebSocket support, same language as most real-time JS libraries) | Good (WebSocket support via Starlette) | Excellent (goroutines make concurrent connections cheap) |
| AI/ML integration | Fair (calls out to Python/ML services; not itself an ML runtime) | Excellent (native home of the Python ML ecosystem — TensorFlow, PyTorch) | Fair (ML ecosystem is far smaller than Python's) |
| Ecosystem / libraries | Very large (npm), mature ORMs (TypeORM, Prisma) | Large and fast-growing, especially for data/ML tooling | Smaller web-framework ecosystem, but excellent standard library |
| Learning curve | Low-medium (TypeScript, structured/opinionated like Angular) | Low (Python is widely taught, FastAPI's docs are excellent) | Medium-high (new language/paradigm for most web teams) |
| Team fit | High — same language (TypeScript/JS) as the React Native frontend, reducing context-switching | Medium — different language from frontend, but same language as the AI microservice | Low — a third language on top of frontend and AI service |
| Maintenance cost | Low (one language across frontend+backend, strong tooling) | Low-medium (one extra language, but shared with AI service) | Medium (smaller hiring pool for Go among student/early-career devs) |

**Assessment:** NestJS is the strongest general-purpose backend choice for FitFlow because it
shares a language with the frontend (TypeScript), which matters for a small team. FastAPI is not
discarded, however — it is the natural choice specifically for the **AI microservice**, since it sits
directly in Python's ML ecosystem. Go is technically excellent but adds a third language with no
corresponding benefit large enough to justify the extra hiring/learning cost for this project's scale.

## 2. Database options

| Criterion | PostgreSQL | MongoDB | Firebase (Firestore/RTDB) | DynamoDB |
|---|---|---|---|---|
| Scalability | Vertical + read replicas; horizontal via sharding (more setup) | Horizontal scaling built-in (sharding) | Fully managed horizontal scaling | Fully managed, near-infinite horizontal scaling |
| Query performance | Excellent for relational/joined queries (workout history, user+plan joins) | Good for document-shaped, denormalized data | Good for simple lookups; weak for complex relational queries | Excellent for key-based access; weak for ad hoc relational queries |
| Health data handling | Strong — relational integrity, constraints, and mature auditing support suit sensitive health-adjacent data | Workable but requires enforcing structure at the application level | Workable, but schema-less nature makes consistent health data validation harder | Workable, similar caveats to Firestore |
| Real-time capability | Requires add-ons (e.g., LISTEN/NOTIFY, or a separate real-time layer) | Change streams available but less turnkey than Firebase | Excellent — real-time sync is a first-class feature | Good with DynamoDB Streams, more setup than Firebase |
| Cost (mid-sized team/startup) | Low (open-source, predictable hosting cost) | Low-medium (open-source, Atlas has a free tier) | Low to start (generous free tier), can grow with usage | Pay-per-request can be economical at low scale, less predictable at high scale |
| Maintainability | High — mature tooling, migrations, widely known | High — mature tooling, but more app-level discipline needed for consistency | Medium — very little ops burden, but vendor lock-in | Medium — low ops burden, AWS-specific expertise needed |

**Assessment:** A single database is not the best fit for FitFlow's mixed needs. **PostgreSQL** is
recommended as the system of record for users, workout plans, and nutrition logs, where
relational integrity and auditability matter (this is effectively health-adjacent data). **Firestore /
Firebase Realtime DB** is recommended specifically for the social feed and presence features,
where real-time sync is the primary requirement and the data is inherently less structured. This
polyglot-persistence approach avoids forcing one database to be good at two very different jobs.

## 3. Authentication & authorization

| Criterion | Firebase Auth | AWS Cognito | Auth0 | Supabase Auth |
|---|---|---|---|---|
| Integration speed | Very fast, especially if already using Firebase for real-time data | Medium (more configuration, AWS-specific concepts) | Fast (excellent docs), but a separate vendor from the rest of the stack | Fast if already using Supabase/Postgres |
| Security / compliance | Good (industry-standard OAuth2/OpenID, SOC2 compliant); GDPR-compliant with correct data-residency configuration | Strong, deep AWS security integration, HIPAA-eligible | Strong, enterprise-grade compliance certifications (SOC2, HIPAA add-on) | Good, improving compliance posture, younger product |
| Cost at mid-scale | Low — generous free tier | Low-medium — pay per MAU, AWS pricing model | Higher — Auth0 costs scale up faster past free tier | Low — bundled with Supabase pricing |
| Social login support | Excellent, broad provider support out of the box | Good, more setup required | Excellent, broad provider support | Good, growing provider support |
| Fit with chosen stack | Excellent — same vendor as the real-time database, reduces integration surface | Requires adopting AWS more broadly | Adds a vendor unrelated to the rest of the stack | Would imply switching primary DB to Supabase Postgres, not chosen here |

**Assessment:** **Firebase Auth** is recommended primarily for integration simplicity and its
natural pairing with the Firestore/Realtime DB layer already chosen for social features — one
fewer vendor relationship for a small team to manage. Auth0 remains the stronger choice if
FitFlow later needs enterprise SSO or has stricter formal compliance requirements (e.g.
HIPAA-level certification) than Firebase currently offers out of the box; this is noted as a
re-evaluation trigger in the ADR.

## 4. Security, real-time, AI integration, cost, and maintainability — consolidated view

- **Security/compliance (GDPR):** PostgreSQL + Firebase Auth + Firestore, all configured with
  encryption at rest/in transit and EU/appropriate data residency settings, meet GDPR
  requirements as used in the original case study. Formal HIPAA certification is not required
  for FitFlow (it is a consumer wellness app, not a covered healthcare entity), but the same
  encryption and access-control practices are applied as good practice for health-adjacent data.
- **Real-time capability:** Covered by Firebase Realtime DB/Firestore for the social feed, and
  NestJS's native WebSocket support for anything server-driven (e.g. live coaching nudges).
- **AI integration:** Covered by a dedicated FastAPI microservice, kept separate from the NestJS
  core API so the ML runtime and its dependencies don't bloat or destabilize the main backend.
- **Cost for a mid-sized team:** All chosen options (NestJS, PostgreSQL, Firebase, FastAPI) have
  low or no licensing cost and generous free/low tiers, appropriate for a startup redesign budget.
- **Maintainability:** Two backend languages (TypeScript for NestJS, Python for FastAPI) is a
  deliberate, minimal increase in complexity — justified because Python is non-negotiable for
  serious ML work, and keeping the ML service isolated limits the blast radius of that complexity.
