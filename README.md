# Nuevo Proyecto Scaffold

[![Version](https://img.shields.io/badge/version-3.0.0-blue.svg)](https://github.com/vdirienzo/nuevo-proyecto-scaffold)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Stars](https://img.shields.io/github/stars/vdirienzo/nuevo-proyecto-scaffold?style=social)](https://github.com/vdirienzo/nuevo-proyecto-scaffold/stargazers)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

> **Netflix-Scale Enterprise Fullstack Scaffold**
>
> Production-ready project generator with enterprise-grade architecture, security, and observability out of the box.

---

## Features

- **Interactive Wizard** - 10-phase guided setup with intelligent recommendations
- **Multi-Scale Ready** - From MVP to Netflix-scale with one command
- **148 Templates** - Battle-tested templates for every component
- **Full Stack Support** - FastAPI, NestJS, Go backends + Next.js, React frontends
- **Security First** - 4-tier security levels from Standard to Zero-Trust
- **Complete Observability** - OpenTelemetry, Sentry, distributed tracing
- **CI/CD Batteries Included** - 5 GitHub Actions workflows ready to deploy
- **Docker Everything** - Dev, test, prod containers + DevContainers
- **Testing Pyramid** - Unit, integration, e2e, contract, load, chaos testing
- **Monorepo Architecture** - Turborepo with optimized builds and caching
- **Feature Flags** - Built-in flag management for gradual rollouts
- **Multi-Auth Support** - JWT, OAuth2, Supabase Auth, Firebase Auth
- **Database Agnostic** - Supabase, PostgreSQL, Firebase, MongoDB
- **Compliance Ready** - GDPR, SOC2, HIPAA templates

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
    Phase2 -.->|MVP, Scale-up, Enterprise, Netflix| Scale
    Phase3 -.->|Backend, Frontend, Database| Stack
    Phase4 -.->|JWT, OAuth2, Supabase, Firebase| Auth
    Phase5 -.->|Unit, Integration, E2E, Contract| Testing
    Phase6 -.->|Standard, Enhanced, Compliance, Zero-Trust| Security
    Phase7 -.->|Docker, K8s, Terraform, GitHub Actions| Infra
    Phase8 -.->|Sentry, OpenTelemetry, Metrics| Observability
    Phase9 -.->|DevContainers, Pre-commit, Hot Reload| DX
    Phase10 -.->|Feature Flags, WebSockets, GraphQL| Features

    Generate --> Output[148 Files Generated]
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
    Enterprise --> Netflix[Netflix]

    MVP -.->|Single Server<br/>SQLite/Postgres<br/>Basic Logging| MVPFeatures
    ScaleUp -.->|Load Balancer<br/>Redis Cache<br/>Managed DB<br/>CI/CD| ScaleFeatures
    Enterprise -.->|Multi-Region<br/>Microservices<br/>K8s<br/>Full Observability| EnterpriseFeatures
    Netflix -.->|Global CDN<br/>Chaos Engineering<br/>Multi-Cloud<br/>Zero-Trust Security| NetflixFeatures

    style MVP fill:#fff9c4
    style ScaleUp fill:#c8e6c9
    style Enterprise fill:#90caf9
    style Netflix fill:#ce93d8
```

---

## Technology Stack

### Backend Options

| Technology | Features | Best For |
|------------|----------|----------|
| **FastAPI** | Async, Type-safe, OpenAPI docs | Python projects, ML APIs, rapid development |
| **NestJS** | Enterprise TypeScript, Modular | Large teams, complex domains |
| **Go** | High performance, Compiled | Microservices, low-latency systems |

### Frontend Options

| Technology | Features | Best For |
|------------|----------|----------|
| **Next.js** | SSR, SSG, API routes, App Router | SEO-critical apps, full-stack |
| **React SPA** | CSR, Fast iteration | Admin panels, internal tools |

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

---

## Template Structure

```
templates/
├── backend/
│   ├── fastapi/
│   │   ├── src-layout/
│   │   │   ├── main.py
│   │   │   ├── config.py
│   │   │   ├── models/
│   │   │   ├── routes/
│   │   │   ├── services/
│   │   │   └── utils/
│   │   ├── auth/
│   │   │   ├── jwt.py
│   │   │   ├── oauth2.py
│   │   │   └── dependencies.py
│   │   ├── database/
│   │   │   ├── supabase.py
│   │   │   ├── postgres.py
│   │   │   └── migrations/
│   │   ├── tests/
│   │   │   ├── unit/
│   │   │   ├── integration/
│   │   │   └── e2e/
│   │   └── docker/
│   │       ├── Dockerfile
│   │       ├── Dockerfile.dev
│   │       └── Dockerfile.prod
│   ├── nestjs/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   ├── main.ts
│   │   │   ├── auth/
│   │   │   ├── users/
│   │   │   └── common/
│   │   ├── test/
│   │   └── docker/
│   └── go/
│       ├── cmd/
│       ├── internal/
│       ├── pkg/
│       └── docker/
├── frontend/
│   ├── nextjs/
│   │   ├── app/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   └── api/
│   │   ├── components/
│   │   │   ├── ui/
│   │   │   └── features/
│   │   ├── lib/
│   │   │   ├── auth.ts
│   │   │   ├── api.ts
│   │   │   └── utils.ts
│   │   └── tests/
│   └── react-spa/
│       ├── src/
│       │   ├── App.tsx
│       │   ├── components/
│       │   ├── hooks/
│       │   └── services/
│       └── tests/
├── infrastructure/
│   ├── docker/
│   │   ├── docker-compose.yml
│   │   ├── docker-compose.dev.yml
│   │   ├── docker-compose.prod.yml
│   │   └── docker-compose.test.yml
│   ├── kubernetes/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── ingress.yaml
│   │   ├── configmap.yaml
│   │   └── secrets.yaml
│   ├── terraform/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── modules/
│   └── github-actions/
│       ├── ci.yml
│       ├── cd.yml
│       ├── security.yml
│       ├── tests.yml
│       └── release.yml
├── observability/
│   ├── sentry/
│   │   └── sentry.config.js
│   ├── opentelemetry/
│   │   ├── instrumentation.ts
│   │   └── config.yaml
│   ├── prometheus/
│   │   └── prometheus.yml
│   └── grafana/
│       └── dashboards/
├── security/
│   ├── oauth2/
│   │   └── providers.ts
│   ├── rbac/
│   │   ├── roles.ts
│   │   └── permissions.ts
│   ├── rate-limiting/
│   │   └── middleware.ts
│   └── secrets/
│       ├── .env.example
│       └── secrets.yaml.example
├── testing/
│   ├── unit/
│   │   └── jest.config.js
│   ├── integration/
│   │   └── pytest.ini
│   ├── e2e/
│   │   └── playwright.config.ts
│   ├── contract/
│   │   └── pact.config.js
│   └── load/
│       └── k6-script.js
├── compliance/
│   ├── gdpr/
│   │   ├── data-retention.md
│   │   └── consent-management.ts
│   ├── soc2/
│   │   └── audit-logging.ts
│   └── hipaa/
│       └── phi-encryption.ts
└── docs/
    ├── README.md
    ├── CONTRIBUTING.md
    ├── ARCHITECTURE.md
    ├── DEPLOYMENT.md
    └── API.md

Total: 148 templates across 25 categories
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
    Chaos[Chaos Engineering - Netflix Tier]
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
| **Netflix** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

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

### Hot Reload
- Backend: uvicorn --reload, nodemon
- Frontend: Next.js Fast Refresh, Vite HMR

### Pre-commit Hooks
- Code formatting (ruff, prettier)
- Linting (mypy, eslint)
- Security scanning (bandit, semgrep)
- Test running (fast tests only)

### Documentation
- API documentation (OpenAPI, GraphQL Playground)
- Architecture Decision Records (ADRs)
- Deployment guides
- Troubleshooting guides

---

## Additional Features

### Feature Flags
- **LaunchDarkly** integration
- **Split.io** integration
- **Custom Flag Service** option
- Gradual rollouts
- A/B testing support

### WebSockets
- Real-time communication
- Socket.IO integration
- Redis pub/sub for scaling

### GraphQL
- Apollo Server
- Type-safe schemas
- GraphQL Playground
- Subscriptions support

### Message Queues
- **RabbitMQ** - Traditional messaging
- **Redis Streams** - Lightweight queues
- **Kafka** - Event streaming (Enterprise+)

### Storage
- **S3** - Object storage
- **Cloudinary** - Image optimization
- **Supabase Storage** - Integrated storage

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

- [ ] GraphQL federation support
- [ ] gRPC service templates
- [ ] Rust backend option
- [ ] Mobile app templates (React Native, Flutter)
- [ ] ML/AI service templates
- [ ] Blockchain integration templates
- [ ] Edge computing templates (Cloudflare Workers, Deno Deploy)

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

## Author

**Homero Thompson del Lago del Terror**

---

## Acknowledgments

- Inspired by Netflix's engineering practices
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
