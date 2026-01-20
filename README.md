# Nuevo Proyecto Scaffold

[![Version](https://img.shields.io/badge/version-6.2.0-blue.svg)](https://github.com/vdirienzo/nuevo-proyecto-scaffold)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Stars](https://img.shields.io/github/stars/vdirienzo/nuevo-proyecto-scaffold?style=social)](https://github.com/vdirienzo/nuevo-proyecto-scaffold/stargazers)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

> **Hyperscale-Scale Enterprise Fullstack Scaffold**
>
> Production-ready project generator with enterprise-grade architecture, security, and observability out of the box.

---

## Features

- **Interactive Wizard** - 11-phase guided setup with intelligent recommendations
- **Multi-Scale Ready** - From MVP to Hyperscale-scale with one command
- **972 Templates** - Battle-tested templates for every component and scenario
- **5 Design Systems** - Geist, Glassmorphism, Neumorphism, Claymorphism, Neo-Brutalism with React + Vue components
- **Full Stack Support** - FastAPI, NestJS, Go, Rust (Actix/Rocket) backends + Next.js, React SPA, Vue.js 3, Angular 17+, jQuery frontends
- **Mobile Apps** - React Native and Flutter cross-platform templates
- **Desktop Apps** - Tauri v2 (Rust backend + web frontend) for Windows, macOS, Linux with 3-10MB bundles
- **Edge Computing** - Cloudflare Workers and Deno Deploy for global edge deployment
- **WebAssembly** - Rust-WASM and AssemblyScript modules for high-performance computation
- **Blockchain/Web3** - Web3.js and Ethers.js integration with smart contracts
- **Security First** - 4-tier security levels from Standard to Zero-Trust
- **Complete Observability** - OpenTelemetry, Prometheus, Grafana, Jaeger, Sentry
- **CI/CD Batteries Included** - 5 GitHub Actions workflows ready to deploy
- **Docker Everything** - Dev, test, prod containers + DevContainers + Storybook
- **Testing Pyramid** - Unit, integration, e2e, contract, load, chaos testing
- **Monorepo Architecture** - Turborepo with optimized builds and caching
- **Feature Flags** - Built-in flag management for gradual rollouts
- **Multi-Auth Support** - JWT, OAuth2, Auth0, Clerk, SAML SSO, MFA/2FA, Supabase, Firebase
- **Authorization** - RBAC and ABAC patterns built-in
- **Database Agnostic** - Supabase, PostgreSQL, Firebase, MongoDB
- **Cache Ready** - Redis and Dragonfly support with scaling patterns
- **API Styles** - REST, GraphQL, gRPC, tRPC (end-to-end type-safe)
- **Developer Experience** - DevContainers, Storybook, MSW (Mock Service Worker), hot reload
- **Compliance Ready** - GDPR, SOC2, HIPAA templates
- **Extras** - AI/LLM integration, Payments (Stripe), Search (Elasticsearch), Email, Jobs, i18n

---

## What's New

### v6.2.0 - Design Systems Edition 🎨

**99 New Templates** - Expanding from 873 to **972 templates** with 5 complete design systems.

**🎨 Design Systems (86 templates)**
- **Geist** (12) - Vercel aesthetic: black background, gradient accents, ultra-clean typography
- **Glassmorphism** (13) - Frosted glass: backdrop-blur, glowing borders, futuristic dark mode
- **Neumorphism** (15) - Soft UI: inset/outset shadows, tactile feel, calm aesthetic
- **Claymorphism** (15) - Clay/plasticine: organic shapes, pastel colors, playful animations
- **Neo-Brutalism** (16) - Bold & chunky: loud colors, thick borders, asymmetric layouts
- **Shared utilities** (10) - React hooks, Vue composables, design tokens, animations

Each system includes:
- Tailwind CSS configuration
- React components (Button, Card, Input, Modal, Navbar, etc.)
- Vue 3 components (Composition API)
- Landing page example
- Full documentation

---

### v6.1.0 - Tauri Desktop Edition 🖥️

**74 New Templates** - Expanding from 828 to **873 templates** with desktop app support.

**🖥️ Tauri Desktop (74 templates)**
- **Rust Backend** - Secure IPC commands, SQLite database, auth services
- **Frontend Integration** - React hooks, Vue composables, TypeScript API wrapper
- **Plugins** - Auto-updater, persistent store, logging, window state
- **Security** - Capability-based permissions, CSP, crypto utilities
- **Database** - SQLite with sqlx, migrations, seeds
- **Testing** - Rust integration tests, Playwright E2E
- **CI/CD** - Multi-platform builds (Windows, macOS, Linux)

---

### v6.0.0 - Next Generation Release 🚀

**294 New Templates** - Expanding from 534 to **828 templates** with cutting-edge technologies.

**🦀 Rust Backend (41 templates)**
- **Actix-web** - Maximum performance async web framework
- **Rocket** - Type-safe routing with ergonomic API
- Features: JWT auth, sqlx PostgreSQL, OpenAPI docs, Docker multi-stage

**📱 Mobile Apps (83 templates)**
- **React Native** (46 templates) - Redux Toolkit, React Navigation 6, TypeScript
- **Flutter** (37 templates) - Riverpod 2.0, GoRouter, Material 3, Clean Architecture

**🌐 Edge Computing (69 templates)**
- **Cloudflare Workers** (27 templates) - KV, D1, R2, Durable Objects, Queues
- **Deno Deploy** (42 templates) - Deno KV, Fresh framework, Oak/Hono

**🔧 WebAssembly (27 templates)**
- **Rust → WASM** - wasm-pack, wasm-bindgen, crypto/image processing
- **AssemblyScript** - TypeScript-like syntax to WASM
- Integration examples for React, Vanilla JS, Node.js

**⛓️ Blockchain/Web3 (74 templates)**
- **Web3.js** (35 templates) - Hardhat, ERC-20/721, MetaMask, WalletConnect
- **Ethers.js** (39 templates) - Foundry, wagmi, RainbowKit, Staking, Governance

---

### v5.0.0

**114 New Frontend Templates** - Expanding frontend options from 2 to 6 frameworks.

**New Frontend Options**
- **Vue.js 3** - Composition API + Pinia state management + Vite (42 templates)
- **Angular 17+** - Standalone components + Signals + NgRx (43 templates)
- **jQuery** - Bootstrap 5 + Parcel for rapid prototyping (29 templates)

---

### v4.0.0 - Major Additions

**534 Templates** - Nearly 3.6x expansion from 148 to 534 templates covering every modern development scenario.

**New Backend Options**
- Go with Gin framework for high-performance APIs
- Enhanced Go standard library templates
- Python Strawberry for GraphQL

**Frontend Options**
- React SPA with Vite for blazing-fast development
- Enhanced Next.js templates with App Router best practices

**API Styles**
- GraphQL (NestJS, Strawberry, Apollo)
- gRPC (Go, Python, TypeScript)
- tRPC for end-to-end type-safety

**Authentication & Authorization**
- Auth0 integration (Enterprise SSO, SAML)
- Clerk integration (Modern auth UX)
- SAML SSO support
- MFA/2FA (TOTP, SMS, Email)
- RBAC and ABAC patterns

**Observability**
- Complete OpenTelemetry stack
- Prometheus metrics
- Grafana dashboards (8 pre-built)
- Jaeger distributed tracing

**Developer Experience**
- Storybook integration for component development
- MSW (Mock Service Worker) for API mocking
- Enhanced DevContainers
- Hot reload for all stacks

**Extras**
- AI/LLM integrations (OpenAI, Claude, AWS Bedrock)
- Stripe payments integration
- Elasticsearch and Typesense for search
- SendGrid and Resend for email
- BullMQ and Celery for background jobs
- Feature flags (LaunchDarkly, Split.io)
- i18n support (Next.js, react-i18next)

**Cache**
- Dragonfly support (25x faster than Redis)
- Enhanced Redis patterns

### 📊 By the Numbers

| Metric | v3.0.0 | v6.2.0 | Change |
|--------|--------|--------|--------|
| **Total Templates** | 148 | 972 | +557% |
| **Backend Options** | 3 | 4 | +33% |
| **Frontend Options** | 2 | 6 | +200% |
| **Auth Providers** | 2 | 5 | +150% |
| **API Styles** | 1 | 4 | +300% |
| **Template Categories** | 25 | 35 | +40% |

---

## Quick Start

```bash
# Install Claude Code (if not already installed)
curl -sSL https://anthropic.com/install.sh | bash

# Run the wizard
cd /home/user/projects
claude-code

# Inside Claude Code, run:
/nuevo-proyecto
```

The interactive wizard will guide you through 10 phases to create your perfect project.

---

## Wizard Flow

```mermaid
graph TD
    Start[Start Wizard] --> Phase1[Phase 1: Metadata]
    Phase1 --> Phase2[Phase 2: Scale & Compliance]
    Phase2 --> Phase3[Phase 3: Stack Selection]
    Phase3 --> Phase4[Phase 4: Auth & Authorization]
    Phase4 --> Phase5[Phase 5: Testing Strategy]
    Phase5 --> Phase6[Phase 6: Security]
    Phase6 --> Phase7[Phase 7: Infrastructure & Deployment]
    Phase7 --> Phase8[Phase 8: Observability]
    Phase8 --> Phase9[Phase 9: Developer Experience]
    Phase9 --> Phase10[Phase 10: Additional Features]
    Phase10 --> Generate[Generate Project]

    Phase1 -.->|Project name, description| Metadata
    Phase2 -.->|MVP, Scale-up, Enterprise, Hyperscale| Scale
    Phase3 -.->|Backend, Frontend, Database, Cache| Stack
    Phase4 -.->|JWT, OAuth2, Auth0, Clerk, SAML, MFA| Auth
    Phase5 -.->|Unit, Integration, E2E, Contract, Load| Testing
    Phase6 -.->|Standard, Enhanced, Compliance, Zero-Trust| Security
    Phase7 -.->|Docker, K8s, Terraform, GitHub Actions| Infra
    Phase8 -.->|OpenTelemetry, Prometheus, Grafana, Jaeger| Observability
    Phase9 -.->|DevContainers, Storybook, MSW, Hot Reload| DX
    Phase10 -.->|API Styles, AI/LLM, Payments, Search, i18n| Features

    Generate --> Output[534 Files Generated]
    Output --> Done[Project Ready]

    style Start fill:#e1f5ff
    style Generate fill:#c8e6c9
    style Done fill:#4caf50,color:#fff
```

---

## Scale Tiers

```mermaid
graph LR
    MVP[MVP] --> ScaleUp[Scale-up]
    ScaleUp --> Enterprise[Enterprise]
    Enterprise --> Hyperscale[Hyperscale]

    MVP -.->|Single Server<br/>SQLite/Postgres<br/>Basic Logging| MVPFeatures
    ScaleUp -.->|Load Balancer<br/>Redis Cache<br/>Managed DB<br/>CI/CD| ScaleFeatures
    Enterprise -.->|Multi-Region<br/>Microservices<br/>K8s<br/>Full Observability| EnterpriseFeatures
    Hyperscale -.->|Global CDN<br/>Chaos Engineering<br/>Multi-Cloud<br/>Zero-Trust Security| HyperscaleFeatures

    style MVP fill:#fff9c4
    style ScaleUp fill:#c8e6c9
    style Enterprise fill:#90caf9
    style Hyperscale fill:#ce93d8
```

---

## Technology Stack

### Backend Options

| Technology | Features | Best For |
|------------|----------|----------|
| **FastAPI** | Async, Type-safe, OpenAPI docs | Python projects, ML APIs, rapid development |
| **NestJS** | Enterprise TypeScript, Modular | Large teams, complex domains |
| **Go (Standard)** | High performance, Compiled, stdlib HTTP | Microservices, low-latency systems |
| **Gin (Go)** | Fast HTTP framework, Middleware | High-throughput APIs, REST services |

### Frontend Options

| Technology | Features | Best For |
|------------|----------|----------|
| **Next.js** | SSR, SSG, API routes, App Router | SEO-critical apps, full-stack, production |
| **React SPA (Vite)** | CSR, Fast HMR, Modern tooling | Admin panels, internal tools, dashboards |
| **Vue.js 3** | Composition API, Pinia, Vite | Progressive apps, component-driven design |
| **Angular 17+** | Standalone, Signals, NgRx | Enterprise apps, large teams, complex domains |
| **jQuery** | Bootstrap 5, Parcel, Simple setup | Legacy support, rapid prototyping, small projects |

### Desktop Options

| Technology | Features | Best For |
|------------|----------|----------|
| **Tauri** | Rust backend, 3-10MB bundles, secure IPC | Modern desktop apps, cross-platform |
| **Electron** | Node.js backend, larger bundles (40-200MB) | Legacy support, requires browser APIs |

### Database Options

| Database | Type | Features |
|----------|------|----------|
| **Supabase** | PostgreSQL + Auth + Realtime | Rapid development, built-in auth |
| **PostgreSQL** | Relational | Traditional apps, complex queries |
| **Firebase** | NoSQL + Auth + Hosting | Mobile-first, real-time apps |
| **MongoDB** | Document DB | Flexible schema, rapid iteration |

### Cache Options

| Cache | Features | Best For |
|-------|----------|----------|
| **Redis** | Key-value, Pub/Sub, Streams | Standard caching, sessions |
| **Dragonfly** | Redis-compatible, 25x faster | High-performance scenarios |

### API Styles

| Style | Features | Best For |
|-------|----------|----------|
| **REST** | Standard HTTP, OpenAPI docs | Public APIs, mobile apps |
| **GraphQL** | Query language, Type-safe | Complex data fetching, flexible clients |
| **gRPC** | Binary protocol, HTTP/2 | Microservices, low-latency communication |
| **tRPC** | End-to-end type-safe, TypeScript | Monorepo, full-stack TypeScript apps |

### Auth Providers

| Provider | Features | Best For |
|----------|----------|----------|
| **Custom JWT** | Self-hosted, Full control | Maximum flexibility, on-premise |
| **Supabase Auth** | PostgreSQL-backed, Row-level security | Integrated with Supabase DB |
| **Firebase Auth** | Google-backed, Social logins | Mobile-first, rapid prototyping |
| **Auth0** | Enterprise SSO, SAML, MFA | Large organizations, compliance |
| **Clerk** | Modern UX, Pre-built components | Fast implementation, great DX |

---

## Template Structure

```
templates/
├── backend/ (185 templates)
│   ├── fastapi/
│   │   ├── src-layout/           # Core structure (8)
│   │   ├── auth/                 # JWT, OAuth2, Auth0, Clerk (12)
│   │   ├── database/             # Supabase, PostgreSQL, MongoDB (10)
│   │   ├── cache/                # Redis, Dragonfly (6)
│   │   ├── api-styles/           # REST, GraphQL, gRPC (15)
│   │   ├── authorization/        # RBAC, ABAC (8)
│   │   ├── mfa/                  # TOTP, SMS, Email (6)
│   │   ├── tests/                # Unit, Integration, E2E (12)
│   │   └── docker/               # Dev, Prod, Test (4)
│   ├── nestjs/
│   │   ├── src/                  # Modules, Controllers (15)
│   │   ├── auth/                 # Passport strategies (10)
│   │   ├── database/             # TypeORM, Prisma (8)
│   │   ├── graphql/              # Schema-first, Code-first (12)
│   │   ├── grpc/                 # Proto definitions (8)
│   │   ├── tests/                # Jest, e2e (10)
│   │   └── docker/               # Containers (4)
│   ├── go-standard/
│   │   ├── cmd/                  # Main applications (5)
│   │   ├── internal/             # Private packages (15)
│   │   ├── pkg/                  # Public libraries (10)
│   │   ├── api/                  # HTTP handlers (8)
│   │   └── docker/               # Multi-stage builds (3)
│   └── go-gin/
│       ├── handlers/             # Gin handlers (10)
│       ├── middleware/           # Auth, CORS, Rate-limit (8)
│       ├── models/               # Data models (6)
│       └── tests/                # Testify suites (6)
├── frontend/ (209 templates)
│   ├── nextjs/
│   │   ├── app/                  # App Router pages (15)
│   │   ├── components/
│   │   │   ├── ui/               # Shadcn/ui components (20)
│   │   │   └── features/         # Feature components (12)
│   │   ├── lib/
│   │   │   ├── auth/             # Auth providers (8)
│   │   │   ├── api/              # API clients (6)
│   │   │   └── utils/            # Utilities (5)
│   │   ├── hooks/                # Custom hooks (8)
│   │   ├── tests/                # Jest, Playwright (8)
│   │   └── storybook/            # Stories (6)
│   ├── react-spa/
│   │   ├── src/
│   │   │   ├── components/       # React components (15)
│   │   │   ├── hooks/            # Custom hooks (6)
│   │   │   ├── services/         # API services (5)
│   │   │   └── store/            # Zustand/Redux (4)
│   │   ├── tests/                # Vitest, Testing Library (6)
│   │   └── msw/                  # Mock Service Worker (3)
│   ├── vue3/
│   │   ├── src/
│   │   │   ├── components/       # Vue components (12)
│   │   │   ├── composables/      # Composition API (8)
│   │   │   ├── stores/           # Pinia stores (5)
│   │   │   └── views/            # Page views (8)
│   │   ├── tests/                # Vitest, Testing Library (6)
│   │   └── vite/                 # Vite config (3)
│   ├── angular/
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   ├── components/   # Standalone components (15)
│   │   │   │   ├── services/     # Injectable services (8)
│   │   │   │   ├── stores/       # NgRx stores (6)
│   │   │   │   └── guards/       # Route guards (4)
│   │   │   └── shared/           # Shared modules (6)
│   │   └── tests/                # Jasmine, Karma (4)
│   └── jquery/
│       ├── src/
│       │   ├── js/               # jQuery modules (10)
│       │   ├── pages/            # HTML pages (8)
│       │   └── components/       # UI components (6)
│       ├── styles/               # Bootstrap customization (3)
│       └── tests/                # QUnit tests (2)
├── api-styles/ (45 templates)
│   ├── graphql/
│   │   ├── nestjs/               # NestJS GraphQL (10)
│   │   ├── strawberry/           # Python Strawberry (8)
│   │   └── apollo/               # Apollo Server (7)
│   ├── grpc/
│   │   ├── go/                   # Go gRPC (6)
│   │   ├── python/               # Python grpcio (5)
│   │   └── typescript/           # TS grpc-js (4)
│   └── trpc/
│       ├── server/               # tRPC server (3)
│       └── client/               # tRPC React client (2)
├── infrastructure/ (48 templates)
│   ├── docker/
│   │   ├── compose/              # Dev, Prod, Test (8)
│   │   ├── dockerfiles/          # Multi-stage (6)
│   │   └── devcontainers/        # VS Code (4)
│   ├── kubernetes/
│   │   ├── base/                 # Deployments, Services (10)
│   │   ├── overlays/             # Staging, Prod (6)
│   │   └── helm/                 # Helm charts (5)
│   ├── terraform/
│   │   ├── aws/                  # ECS, RDS, S3 (6)
│   │   └── gcp/                  # GKE, CloudSQL (3)
│   └── github-actions/
│       ├── ci.yml                # Lint, Test, Build (5)
│       ├── cd.yml                # Deploy workflows (3)
│       └── security.yml          # Semgrep, Trivy (2)
├── observability/ (35 templates)
│   ├── opentelemetry/
│   │   ├── traces/               # Distributed tracing (6)
│   │   ├── metrics/              # Custom metrics (5)
│   │   └── logs/                 # Structured logging (4)
│   ├── prometheus/
│   │   ├── config/               # Prometheus config (3)
│   │   └── exporters/            # Custom exporters (3)
│   ├── grafana/
│   │   ├── dashboards/           # Pre-built dashboards (8)
│   │   └── alerts/               # Alert rules (3)
│   └── jaeger/
│       └── config/               # Jaeger setup (3)
├── auth/ (42 templates)
│   ├── jwt/                      # Custom JWT (8)
│   ├── oauth2/                   # OAuth2 providers (6)
│   ├── auth0/                    # Auth0 integration (6)
│   ├── clerk/                    # Clerk integration (6)
│   ├── saml/                     # SAML SSO (4)
│   ├── mfa/
│   │   ├── totp/                 # Time-based OTP (4)
│   │   ├── sms/                  # SMS 2FA (3)
│   │   └── email/                # Email OTP (3)
│   └── rbac/                     # Role-based access (2)
├── security/ (28 templates)
│   ├── rate-limiting/            # Middleware (4)
│   ├── encryption/               # At-rest, In-transit (6)
│   ├── secrets/                  # Vault, AWS Secrets (5)
│   ├── mtls/                     # Mutual TLS (4)
│   ├── waf/                      # Web Application Firewall (3)
│   └── compliance/
│       ├── gdpr/                 # GDPR compliance (2)
│       ├── soc2/                 # SOC2 audit (2)
│       └── hipaa/                # HIPAA encryption (2)
├── testing/ (32 templates)
│   ├── unit/
│   │   ├── jest/                 # Jest config (4)
│   │   ├── pytest/               # pytest fixtures (4)
│   │   └── go-test/              # Go testing (3)
│   ├── integration/              # API tests (5)
│   ├── e2e/
│   │   ├── playwright/           # Playwright tests (6)
│   │   └── cypress/              # Cypress tests (4)
│   ├── contract/                 # Pact tests (3)
│   └── load/                     # k6, Gatling (3)
├── database/ (25 templates)
│   ├── postgres/
│   │   ├── migrations/           # SQL migrations (5)
│   │   └── seeds/                # Test data (3)
│   ├── mongodb/
│   │   ├── schemas/              # Mongoose schemas (4)
│   │   └── indexes/              # Index definitions (3)
│   ├── supabase/
│   │   ├── rls/                  # Row-level security (4)
│   │   └── functions/            # Edge functions (3)
│   └── redis/
│       ├── cache/                # Cache patterns (2)
│       └── pubsub/               # Pub/Sub (1)
├── extras/ (55 templates)
│   ├── ai-llm/
│   │   ├── openai/               # OpenAI integration (6)
│   │   ├── anthropic/            # Claude integration (5)
│   │   └── bedrock/              # AWS Bedrock (4)
│   ├── payments/
│   │   ├── stripe/               # Stripe integration (8)
│   │   └── webhooks/             # Payment webhooks (3)
│   ├── search/
│   │   ├── elasticsearch/        # Full-text search (6)
│   │   └── typesense/            # Typesense setup (3)
│   ├── email/
│   │   ├── sendgrid/             # SendGrid templates (4)
│   │   └── resend/               # Resend integration (3)
│   ├── jobs/
│   │   ├── bull/                 # BullMQ jobs (4)
│   │   └── celery/               # Celery tasks (3)
│   ├── feature-flags/
│   │   ├── launchdarkly/         # LaunchDarkly (3)
│   │   └── custom/               # Custom flags (2)
│   └── i18n/
│       ├── nextjs/               # Next.js i18n (3)
│       └── react/                # react-i18next (3)
├── devtools/ (18 templates)
│   ├── storybook/
│   │   ├── config/               # Storybook setup (4)
│   │   └── stories/              # Component stories (5)
│   ├── msw/
│   │   ├── handlers/             # Mock handlers (5)
│   │   └── server/               # MSW server (2)
│   └── devcontainers/            # VS Code configs (2)
└── docs/ (12 templates)
    ├── README.md                 # Project README (1)
    ├── CONTRIBUTING.md           # Contribution guide (1)
    ├── ARCHITECTURE.md           # Architecture docs (1)
    ├── DEPLOYMENT.md             # Deploy guide (1)
    ├── API.md                    # API documentation (1)
    ├── SECURITY.md               # Security policy (1)
    ├── CHANGELOG.md              # Version history (1)
    ├── CODE_OF_CONDUCT.md        # Community guidelines (1)
    ├── adrs/                     # Architecture decisions (2)
    └── runbooks/                 # Operational guides (2)

Total: 534 templates across 35 categories
```

---

## Security Features

### Standard Tier
- Environment variable management
- HTTPS enforcement
- Basic input validation
- CORS configuration

### Enhanced Tier
- JWT authentication
- Rate limiting
- SQL injection prevention
- XSS protection
- CSRF tokens

### Compliance Tier
- Audit logging
- GDPR compliance
- SOC2 templates
- HIPAA encryption
- Data retention policies

### Zero-Trust Tier
- mTLS between services
- Service mesh integration
- Secret rotation
- Least privilege access
- Network segmentation
- Runtime threat detection

```mermaid
graph LR
    Standard[Standard] --> Enhanced[Enhanced]
    Enhanced --> Compliance[Compliance]
    Compliance --> ZeroTrust[Zero-Trust]

    Standard -.->|HTTPS<br/>CORS<br/>Input Validation| S
    Enhanced -.->|JWT<br/>Rate Limiting<br/>XSS Protection| E
    Compliance -.->|Audit Logs<br/>GDPR<br/>SOC2| C
    ZeroTrust -.->|mTLS<br/>Service Mesh<br/>Secret Rotation| Z

    style Standard fill:#fff9c4
    style Enhanced fill:#c8e6c9
    style Compliance fill:#90caf9
    style ZeroTrust fill:#f48fb1
```

---

## Testing Pyramid

```mermaid
graph TB
    Chaos[Chaos Engineering - Hyperscale Tier]
    Load[Load Testing - k6, Gatling]
    Contract[Contract Testing - Pact]
    E2E[E2E Tests - Playwright, Cypress]
    Integration[Integration Tests - pytest, Jest]
    Unit[Unit Tests - High Coverage]

    Unit --> Integration
    Integration --> E2E
    E2E --> Contract
    Contract --> Load
    Load --> Chaos

    Unit -.->|70% of tests<br/>Fast, isolated| UnitDesc
    Integration -.->|20% of tests<br/>API + DB| IntDesc
    E2E -.->|10% of tests<br/>User flows| E2EDesc
    Contract -.->|API contracts<br/>Consumer-driven| ContractDesc
    Load -.->|Performance<br/>Stress testing| LoadDesc
    Chaos -.->|Resilience<br/>Failure injection| ChaosDesc

    style Unit fill:#c8e6c9
    style Integration fill:#fff9c4
    style E2E fill:#90caf9
    style Contract fill:#ce93d8
    style Load fill:#ffab91
    style Chaos fill:#f48fb1
```

### Test Coverage by Tier

| Tier | Unit | Integration | E2E | Contract | Load | Chaos |
|------|------|-------------|-----|----------|------|-------|
| **MVP** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Scale-up** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Enterprise** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Hyperscale** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Observability Stack

### Metrics
- **Prometheus** - Time-series metrics
- **Grafana** - Visualization dashboards
- **Custom Metrics** - Business KPIs

### Logging
- **Structured Logging** - JSON format
- **Log Aggregation** - Centralized logs
- **Log Levels** - DEBUG, INFO, WARN, ERROR

### Tracing
- **OpenTelemetry** - Distributed tracing
- **Jaeger** - Trace visualization
- **Trace Sampling** - Performance optimization

### Error Tracking
- **Sentry** - Error monitoring
- **Source Maps** - Frontend debugging
- **Release Tracking** - Version correlation

### Alerting
- **PagerDuty** - On-call management
- **Slack/Discord** - Team notifications
- **Alert Rules** - Threshold-based alerts

---

## Developer Experience

### DevContainers
- Pre-configured development environment
- Consistent across team members
- VS Code integration
- Extensions auto-installed
- Database and services included
- One-click setup

### Hot Reload
- Backend: uvicorn --reload, nodemon, Air (Go)
- Frontend: Next.js Fast Refresh, Vite HMR
- Live reloading for config changes
- CSS hot module replacement

### Storybook
- Component development in isolation
- Interactive documentation
- Visual regression testing
- Accessibility checks built-in
- Mobile viewport testing
- Dark mode support

### Mock Service Worker (MSW)
- API mocking for development
- Same mocks for tests
- Network-level interception
- Works in browser and Node.js
- Realistic delay simulation
- Error scenario testing

### Pre-commit Hooks
- Code formatting (ruff, prettier, gofmt)
- Linting (mypy, eslint, golangci-lint)
- Security scanning (bandit, semgrep)
- Test running (fast tests only)
- Commit message linting
- No secrets committed

### Documentation
- API documentation (OpenAPI, GraphQL Playground)
- Architecture Decision Records (ADRs)
- Component documentation (Storybook)
- Deployment guides
- Troubleshooting runbooks
- Onboarding guides

---

## Additional Features

### API Styles

#### GraphQL
- **NestJS GraphQL** - Schema-first or Code-first
- **Strawberry (Python)** - Type-safe GraphQL with Python
- **Apollo Server** - Standalone GraphQL server
- Type-safe schemas and resolvers
- GraphQL Playground
- Subscriptions support
- Federation ready (Enterprise+)

#### gRPC
- **Go gRPC** - High-performance with Protocol Buffers
- **Python grpcio** - Python gRPC implementation
- **TypeScript grpc-js** - Node.js gRPC client/server
- Proto definitions included
- Bi-directional streaming
- Load balancing ready

#### tRPC
- **End-to-end type-safety** - Share types between frontend/backend
- **React Query integration** - Automatic query management
- **Monorepo optimized** - Perfect for Turborepo projects
- No code generation required
- Built-in error handling

### Authentication & Authorization

#### Auth Providers
- **Custom JWT** - Full control, self-hosted
- **Auth0** - Enterprise SSO, SAML, LDAP
- **Clerk** - Modern auth with pre-built UI
- **Supabase Auth** - Integrated with PostgreSQL
- **Firebase Auth** - Google-backed, mobile-first

#### Multi-Factor Authentication (MFA)
- **TOTP** - Time-based one-time passwords (Google Authenticator, Authy)
- **SMS 2FA** - Text message verification
- **Email OTP** - Email-based codes
- Backup codes generation
- Device trust management

#### Authorization
- **RBAC** - Role-Based Access Control with hierarchies
- **ABAC** - Attribute-Based Access Control for complex policies
- Permission inheritance
- Resource-level permissions
- Audit logging for access decisions

### AI & Machine Learning

#### LLM Integrations
- **OpenAI** - GPT-4, GPT-3.5, embeddings
- **Anthropic Claude** - Claude 3 models
- **AWS Bedrock** - Multi-model access
- Streaming responses
- Token counting and cost tracking
- Retry logic with exponential backoff

### Payments

#### Stripe Integration
- Payment intents
- Subscriptions management
- Customer portal
- Webhook handling
- SCA compliance
- Invoice generation
- Tax calculation

### Search

#### Elasticsearch
- Full-text search
- Faceted search
- Autocomplete
- Fuzzy matching
- Index management

#### Typesense
- Fast and typo-tolerant
- Easy setup
- Lightweight alternative

### Email

#### SendGrid
- Transactional emails
- Template management
- Tracking and analytics

#### Resend
- Modern email API
- React email templates
- Webhook support

### Background Jobs

#### BullMQ (Node.js)
- Redis-backed queues
- Job scheduling
- Retry logic
- Progress tracking
- Job prioritization

#### Celery (Python)
- Distributed task queue
- Periodic tasks
- Task chaining
- Result backend

### Feature Flags
- **LaunchDarkly** integration
- **Split.io** integration
- **Custom Flag Service** option
- Gradual rollouts
- A/B testing support
- Multi-variate testing
- User targeting

### Internationalization (i18n)
- **Next.js i18n** - Built-in routing
- **react-i18next** - Translation management
- Locale detection
- Dynamic imports
- RTL support

### Message Queues
- **RabbitMQ** - Traditional messaging
- **Redis Streams** - Lightweight queues
- **Kafka** - Event streaming (Enterprise+)
- Dead letter queues
- Message retry policies

### Storage
- **S3** - Object storage
- **Cloudinary** - Image optimization
- **Supabase Storage** - Integrated storage
- Presigned URLs
- Upload progress tracking

---

## CI/CD Workflows

### 1. CI Workflow
- Lint, format, type-check
- Run unit tests
- Build Docker images
- Security scanning

### 2. CD Workflow
- Deploy to staging
- Run E2E tests
- Deploy to production
- Health checks

### 3. Security Workflow
- Dependency scanning
- SAST (Semgrep)
- Secret detection
- License compliance

### 4. Test Workflow
- Matrix testing (multiple Python/Node versions)
- Coverage reporting
- Performance benchmarks

### 5. Release Workflow
- Semantic versioning
- Changelog generation
- GitHub releases
- Docker image tagging

---

## Project Structure

```
my-project/
├── apps/
│   ├── web/              # Next.js frontend
│   ├── api/              # Backend service
│   └── admin/            # Admin panel (optional)
├── packages/
│   ├── ui/               # Shared UI components
│   ├── types/            # Shared TypeScript types
│   ├── utils/            # Shared utilities
│   └── config/           # Shared configs
├── infrastructure/
│   ├── docker/
│   ├── kubernetes/
│   └── terraform/
├── .github/
│   └── workflows/
├── docs/
├── scripts/
├── turbo.json
├── package.json
└── README.md
```

---

## Commands

### Development
```bash
# Install dependencies
npm install  # or yarn install

# Start all apps
npm run dev

# Start specific app
npm run dev --filter=web
npm run dev --filter=api

# Build all
npm run build

# Test all
npm run test
```

### Docker
```bash
# Development
docker-compose up

# Production
docker-compose -f docker-compose.prod.yml up

# Tests
docker-compose -f docker-compose.test.yml up --abort-on-container-exit
```

### Kubernetes
```bash
# Apply all manifests
kubectl apply -f infrastructure/kubernetes/

# Check status
kubectl get pods

# View logs
kubectl logs -f deployment/api
```

---

## Environment Variables

### Backend
```bash
DATABASE_URL=postgresql://user:pass@localhost:5432/db
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-secret-key-here
SENTRY_DSN=https://your-sentry-dsn
```

### Frontend
```bash
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

See `.env.example` files in each app for full list.

---

## Deployment

### Vercel (Frontend)
```bash
vercel --prod
```

### Railway (Backend)
```bash
railway up
```

### AWS (Full Stack)
```bash
cd infrastructure/terraform
terraform init
terraform apply
```

### Kubernetes
```bash
kubectl apply -f infrastructure/kubernetes/
```

---

## Monitoring & Debugging

### Local Development
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs
- **Grafana**: http://localhost:3001
- **Prometheus**: http://localhost:9090

### Production
- **Sentry**: Error tracking dashboard
- **Grafana**: Metrics and dashboards
- **Jaeger**: Distributed tracing UI
- **CloudWatch/Datadog**: Log aggregation

---

## Troubleshooting

### Common Issues

**Docker build fails**
```bash
# Clear cache and rebuild
docker-compose build --no-cache
```

**Port already in use**
```bash
# Find and kill process
lsof -ti:8000 | xargs kill -9
```

**Database connection errors**
```bash
# Check database is running
docker-compose ps
# View logs
docker-compose logs db
```

**Tests failing in CI but passing locally**
```bash
# Run tests in same environment as CI
docker-compose -f docker-compose.test.yml up
```

---

## Roadmap

### v5.0.0 (Current) ✅
- [x] Vue.js 3 frontend option (Composition API + Pinia)
- [x] Angular 17+ frontend option (Standalone + Signals)
- [x] jQuery frontend option (Bootstrap 5 + Parcel)
- [x] 534 total templates

### v4.0.0 ✅
- [x] GraphQL support (NestJS, Strawberry, Apollo)
- [x] gRPC service templates (Go, Python, TypeScript)
- [x] tRPC end-to-end type-safety
- [x] Auth providers (Auth0, Clerk, SAML SSO)
- [x] MFA/2FA implementation
- [x] RBAC/ABAC authorization
- [x] AI/LLM integrations (OpenAI, Claude, Bedrock)
- [x] Payments (Stripe)
- [x] Search (Elasticsearch, Typesense)
- [x] Storybook integration
- [x] MSW (Mock Service Worker)
- [x] Go Gin framework
- [x] React SPA with Vite
- [x] Dragonfly cache support
- [x] 420 total templates

### v4.1.0 (Planned - Q2 2026)
- [ ] Rust backend option (Actix-web, Rocket)
- [ ] Mobile app templates (React Native, Flutter)
- [ ] WebAssembly modules
- [ ] Edge computing templates (Cloudflare Workers, Deno Deploy)
- [ ] Blockchain integration templates (Web3.js, Ethers.js)

### v5.0.0 (Planned - Q4 2026)
- [ ] AI-powered code generation
- [ ] Visual project builder (no-code wizard)
- [ ] Plugin marketplace
- [ ] Custom template creator
- [ ] Real-time collaboration features
- [ ] Multi-tenancy templates

---

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

### Quick Start
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

## Changelog

### [6.2.0] - 2026-01-20

#### Added
- **99 new templates** (873 → 972 total)
- 5 complete UI Design Systems with React + Vue components:
  - Geist (Vercel aesthetic) - 12 templates
  - Glassmorphism (frosted glass) - 13 templates
  - Neumorphism (soft UI) - 15 templates
  - Claymorphism (playful clay) - 15 templates
  - Neo-Brutalism (bold chunky) - 16 templates
  - Shared utilities (hooks, tokens) - 10 templates
- Landing page examples for each design system
- Design tokens and Tailwind configurations
- New wizard question: design_system
- Central design system documentation and comparison guide

### [6.1.0] - 2026-01-20

#### Added
- **74 new desktop templates** (828 → 873 total)
- Tauri v2 desktop application framework
- Rust backend with IPC commands (app, auth, files, system)
- SQLite database with sqlx and migrations
- Frontend integration (React hooks, Vue composables)
- Tauri plugins (updater, store, log, window-state)
- Security templates (capabilities, CSP, crypto)
- Playwright E2E tests for desktop
- Multi-platform CI/CD (Windows, macOS, Linux)
- New wizard question: desktop_platform

### [5.0.0] - 2026-01-19

#### Added
- **114 new frontend templates** (420 → 534 total)
- Vue.js 3 frontend option with Composition API, Pinia state management, and Vite (42 templates)
- Angular 17+ frontend option with Standalone components, Signals, and NgRx (43 templates)
- jQuery frontend option with Bootstrap 5 and Parcel bundler (29 templates)

#### Changed
- Expanded frontend options from 2 to 6 frameworks
- Updated Frontend Options table with 3 new frameworks and their features

### [4.0.0] - 2026-01-19

#### Added
- **272 new templates** across 10 new categories (148 → 420 total)
- Go Gin framework backend option
- React SPA with Vite frontend option
- MongoDB database support
- Dragonfly cache support (Redis-compatible, 25x faster)
- GraphQL templates (NestJS, Strawberry, Apollo)
- gRPC templates (Go, Python, TypeScript)
- tRPC end-to-end type-safe API
- Auth0 authentication provider
- Clerk authentication provider
- SAML SSO support
- MFA/2FA templates (TOTP, SMS, Email)
- RBAC and ABAC authorization patterns
- Complete OpenTelemetry observability stack
- Prometheus metrics collection
- Grafana dashboards (8 pre-built)
- Jaeger distributed tracing
- Storybook component development
- MSW (Mock Service Worker) for API mocking
- AI/LLM integrations (OpenAI, Anthropic Claude, AWS Bedrock)
- Stripe payments integration
- Elasticsearch and Typesense search
- SendGrid and Resend email services
- BullMQ and Celery background jobs
- LaunchDarkly and Split.io feature flags
- Next.js and react-i18next i18n support

#### Changed
- Enhanced DevContainers with auto-setup
- Improved hot reload for all backend frameworks
- Updated Next.js templates to App Router best practices
- Expanded testing templates with more scenarios
- Enhanced security templates with Zero-Trust patterns

#### Improved
- Documentation structure with 35 categories
- Template organization for better discoverability
- CI/CD workflows with parallel execution
- Docker multi-stage builds optimization

### [3.0.0] - 2025-12-15

#### Added
- Initial release with 148 templates
- FastAPI, NestJS, Go backend support
- Next.js frontend support
- Supabase and PostgreSQL database options
- Firebase integration
- JWT and OAuth2 authentication
- Basic observability (Sentry)
- Docker and Kubernetes infrastructure
- GitHub Actions CI/CD
- Testing pyramid (unit, integration, e2e)
- GDPR, SOC2, HIPAA compliance templates

---

## Author

**Homero Thompson del Lago del Terror**

---

## Acknowledgments

- Inspired by Hyperscale's engineering practices
- Built with best practices from Google, Meta, and Amazon
- Community-driven template contributions
- Battle-tested in production environments

---

## Support

- **Documentation**: [Full docs](https://docs.example.com)
- **Issues**: [GitHub Issues](https://github.com/vdirienzo/nuevo-proyecto-scaffold/issues)
- **Discussions**: [GitHub Discussions](https://github.com/vdirienzo/nuevo-proyecto-scaffold/discussions)
- **Discord**: [Join our community](https://discord.gg/example)

---

**Made with ❤️ for developers who ship fast without breaking things**
