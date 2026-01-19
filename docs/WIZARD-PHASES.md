# Wizard Phases - Complete Guide

Detailed documentation of all wizard phases, options, and decision-making guidance.

## Table of Contents

- [Overview](#overview)
- [Phase 1: Project Name](#phase-1-project-name)
- [Phase 2: Scale Selection](#phase-2-scale-selection)
- [Phase 3: Backend Framework](#phase-3-backend-framework)
- [Phase 4: Frontend Framework](#phase-4-frontend-framework)
- [Phase 5: Database & Auth](#phase-5-database--auth)
- [Phase 6: State Management](#phase-6-state-management)
- [Phase 7: Background Jobs](#phase-7-background-jobs)
- [Phase 8: Real-time Features](#phase-8-real-time-features)
- [Phase 9: File Storage](#phase-9-file-storage)
- [Phase 10: Email Service](#phase-10-email-service)
- [Phase 11: Monitoring & Observability](#phase-11-monitoring--observability)
- [Phase 12: Deployment Strategy](#phase-12-deployment-strategy)
- [Phase 13: Additional Features](#phase-13-additional-features)
- [Scale-Based Defaults](#scale-based-defaults)
- [Decision Trees](#decision-trees)
- [Project Type Examples](#project-type-examples)

## Overview

The wizard consists of 13 phases that progressively build your project configuration. Each phase offers options tailored to your project scale and previous selections.

**Navigation:**
- Use arrow keys to move between options
- Press Enter to select
- Type 'back' to return to previous phase
- Type 'quit' to exit wizard

**Recommendations:**
- Yellow indicators show recommended options based on scale
- You can always override recommendations
- Hover/select options for detailed descriptions

## Phase 1: Project Name

**Question:** "What is your project name?"

### Validation Rules

- **Format:** lowercase letters, numbers, hyphens only
- **Pattern:** `^[a-z0-9-]+$`
- **Length:** 3-50 characters
- **Reserved names:** Cannot use: test, example, demo, sample

### Valid Examples

```
✓ my-saas-app
✓ ecommerce-api
✓ user-dashboard
✓ data-pipeline-v2
✓ company-internal-tool
```

### Invalid Examples

```
✗ My App          (uppercase, space)
✗ my_app          (underscore)
✗ MyApp           (camelCase)
✗ my.app          (dot)
✗ ab              (too short)
```

### Usage Impact

The project name is used for:

| Context | Transformation | Example |
|---------|---------------|---------|
| Directory name | As-is | `my-saas-app/` |
| Python package | snake_case | `my_saas_app` |
| TypeScript package | As-is | `@org/my-saas-app` |
| Docker image | As-is | `my-saas-app:latest` |
| Database name | Remove hyphens | `mysaasapp` |

## Phase 2: Scale Selection

**Question:** "What is your project scale?"

### Scale Options

#### Micro
- **Team size:** 1 developer
- **Expected traffic:** < 1,000 users/day
- **Complexity:** Simple, single-purpose
- **Budget:** Minimal
- **Examples:** MVPs, experiments, personal projects

**Default selections:**
- Single deployment target
- Minimal monitoring (Basic logs)
- In-memory state management
- Local file storage
- No background jobs
- Single database

#### Small
- **Team size:** 2-5 developers
- **Expected traffic:** < 50,000 users/day
- **Complexity:** Moderate, focused features
- **Budget:** Low to moderate
- **Examples:** Startups, internal tools, small SaaS

**Default selections:**
- Simple deployment (Vercel/Railway)
- Basic monitoring (Sentry)
- Redis for caching
- Managed services (Supabase, S3)
- Optional background jobs
- Single region

#### Medium
- **Team size:** 5-20 developers
- **Expected traffic:** < 500,000 users/day
- **Complexity:** Multiple features, integrations
- **Budget:** Moderate
- **Examples:** Growing products, B2B platforms

**Default selections:**
- Container-based deployment (Docker)
- Enhanced monitoring (DataDog/New Relic)
- Redis cluster
- CDN for assets
- Background job processing
- Primary + replica database
- Feature flags

#### Large
- **Team size:** 20-50 developers
- **Expected traffic:** < 5 million users/day
- **Complexity:** Complex, many integrations
- **Budget:** Substantial
- **Examples:** Established products, high-traffic platforms

**Default selections:**
- Kubernetes orchestration
- Full observability stack (OpenTelemetry)
- Distributed caching
- Multi-region deployment
- Message queues
- Database sharding
- Advanced CI/CD
- A/B testing

#### Enterprise
- **Team size:** 50+ developers
- **Expected traffic:** 5M+ users/day
- **Complexity:** Very complex, microservices
- **Budget:** High
- **Examples:** Large organizations, global platforms

**Default selections:**
- Full microservices architecture
- Custom observability platform
- Multi-region active-active
- Event-driven architecture
- Data lakes
- Advanced security
- Compliance tooling
- Internal developer platform

### Recommendation Helper

The wizard provides recommendations based on:

```
IF team_size == 1 AND traffic < 10K THEN
  RECOMMEND: Micro

ELSE IF team_size <= 5 AND traffic < 100K THEN
  RECOMMEND: Small

ELSE IF team_size <= 20 AND traffic < 1M THEN
  RECOMMEND: Medium

ELSE IF team_size <= 50 AND traffic < 10M THEN
  RECOMMEND: Large

ELSE
  RECOMMEND: Enterprise
```

**Override when:**
- Rapid growth expected (start one scale higher)
- Complex domain (scale up for architecture)
- Regulatory requirements (scale up for compliance)
- Budget constraints (scale down, plan to upgrade)

## Phase 3: Backend Framework

**Question:** "Choose your backend framework"

### Option 1: FastAPI (Python)

**Best for:**
- Rapid API development
- Data science/ML integration
- Python-first teams
- Async-heavy workloads

**Generated:**
- FastAPI with async SQLModel
- Pydantic for validation
- Alembic migrations
- JWT authentication (if not using Supabase)
- pytest test suite
- Ruff for linting
- mypy for type checking

**Project structure:**
```
backend/
├── src/
│   ├── main.py
│   ├── config.py
│   ├── models/
│   ├── routes/
│   ├── services/
│   └── utils/
├── tests/
├── alembic/
└── pyproject.toml
```

**Dependencies:**
- fastapi
- uvicorn
- sqlmodel
- pydantic-settings
- python-jose (JWT)
- httpx
- loguru

**Scale considerations:**
- Micro-Medium: Single uvicorn instance
- Large: Gunicorn with multiple workers
- Enterprise: Kubernetes with autoscaling

### Option 2: NestJS (TypeScript)

**Best for:**
- Enterprise applications
- TypeScript teams
- Microservices architecture
- Complex business logic

**Generated:**
- NestJS with dependency injection
- TypeORM or Drizzle ORM
- Passport authentication
- Jest test suite
- ESLint + Prettier
- OpenAPI documentation

**Project structure:**
```
backend/
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   ├── modules/
│   ├── services/
│   └── guards/
├── test/
└── package.json
```

**Dependencies:**
- @nestjs/core
- @nestjs/common
- @nestjs/typeorm
- @nestjs/passport
- drizzle-orm (or typeorm)
- class-validator

**Scale considerations:**
- Micro-Small: Single instance
- Medium-Large: PM2 cluster mode
- Enterprise: Kubernetes with microservices

### Option 3: API-less (Supabase Direct)

**Best for:**
- Rapid prototyping
- Simple CRUD applications
- Frontend-heavy apps
- Minimal backend logic

**Generated:**
- Next.js with Supabase client
- Row-level security (RLS) policies
- Supabase functions for complex logic
- Direct database access from frontend

**Note:** No custom backend code generated. All logic in:
- Supabase Edge Functions
- Database triggers/functions
- Next.js API routes

**Scale considerations:**
- Suitable for Micro-Small
- For Medium+: Consider adding custom backend

### Comparison Table

| Aspect | FastAPI | NestJS | API-less |
|--------|---------|--------|----------|
| Language | Python | TypeScript | N/A |
| Learning curve | Low | Medium | Very low |
| Type safety | Via mypy | Native | Via TypeScript |
| Performance | High (async) | High | N/A |
| Ecosystem | Data/ML rich | Enterprise rich | Supabase |
| Best scale | Micro-Large | Small-Enterprise | Micro-Small |

## Phase 4: Frontend Framework

**Question:** "Choose your frontend framework"

### Option 1: Next.js (React)

**Best for:**
- Full-stack applications
- SEO requirements
- Server-side rendering
- Modern React development

**Generated:**
- Next.js 14+ with App Router
- TypeScript
- TailwindCSS
- shadcn/ui components
- React Query for data fetching
- Zustand for state management
- Vitest + React Testing Library

**Project structure:**
```
apps/web/
├── src/
│   ├── app/
│   ├── components/
│   ├── lib/
│   └── hooks/
├── public/
└── package.json
```

**Dependencies:**
- next
- react
- typescript
- tailwindcss
- @tanstack/react-query
- zustand
- zod (validation)

**Rendering options:**
- Static generation (SSG)
- Server-side rendering (SSR)
- Incremental static regeneration (ISR)
- Client-side rendering (CSR)

**Scale considerations:**
- Micro-Small: Vercel deployment
- Medium-Large: Self-hosted with CDN
- Enterprise: Edge deployment

### Option 2: None (API-only project)

Select this if:
- Building API/backend service only
- Mobile app will consume API
- Frontend is separate project
- Microservices architecture

**Generated:**
- Backend only
- OpenAPI documentation
- API testing examples
- CORS configuration

## Phase 5: Database & Auth

**Question:** "Choose database and authentication strategy"

### Option 1: Supabase (Database + Auth)

**Includes:**
- PostgreSQL database (managed)
- Built-in authentication
- Row-level security (RLS)
- Realtime subscriptions
- Storage buckets
- Edge functions

**Best for:**
- Rapid development
- Small-Medium scale
- Managed infrastructure preference
- Built-in auth requirements

**Authentication features:**
- Email/password
- OAuth providers (Google, GitHub, etc.)
- Magic links
- Phone authentication
- JWT tokens

**Generated:**
- Supabase client configuration
- Environment variables
- RLS policy examples
- Auth guards/middleware

**Configuration:**
```env
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=eyJhbG...
SUPABASE_SERVICE_KEY=eyJhbG...
```

**Cost:** Free tier available, scales with usage

### Option 2: PostgreSQL + JWT

**Includes:**
- Self-hosted or managed PostgreSQL
- Custom JWT authentication
- Password hashing (bcrypt)
- Refresh tokens
- Role-based access control (RBAC)

**Best for:**
- Full control over auth
- Custom authentication logic
- On-premise requirements
- Cost optimization

**Generated:**
- Database configuration
- Migration files
- JWT utilities
- Auth routes/endpoints
- Password reset flow
- User management

**PostgreSQL setup:**
```yaml
# docker-compose.yml
db:
  image: postgres:16-alpine
  environment:
    POSTGRES_DB: myapp
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
```

**JWT implementation:**
- Access tokens (short-lived)
- Refresh tokens (long-lived)
- Token rotation
- Blacklist support

### Option 3: Firebase

**Includes:**
- Firestore database
- Firebase Authentication
- Cloud Functions
- Firebase Storage
- Analytics

**Best for:**
- Mobile + web apps
- Real-time collaboration
- Google ecosystem preference
- Rapid prototyping

**Authentication features:**
- Email/password
- OAuth providers
- Phone authentication
- Anonymous auth
- Custom tokens

**Generated:**
- Firebase SDK configuration
- Security rules
- Auth provider setup
- Firestore queries

**Cost:** Free tier available, pay-as-you-go

### Comparison Table

| Feature | Supabase | PostgreSQL+JWT | Firebase |
|---------|----------|----------------|----------|
| Setup time | Minutes | Hours | Minutes |
| Customization | Medium | High | Low |
| Cost (small) | Free | $5-20/mo | Free |
| Cost (large) | $25+/mo | $50+/mo | $100+/mo |
| Real-time | ✓ | Manual | ✓ |
| SQL access | ✓ | ✓ | ✗ |
| Vendor lock | Medium | None | High |

## Phase 6: State Management

**Question:** "Choose state management solution"

### Option 1: Redis

**Best for:**
- Caching
- Session storage
- Rate limiting
- Real-time features
- Distributed systems

**Use cases:**
- API response caching
- User session management
- Pub/sub messaging
- Temporary data storage
- Leaderboards/counters

**Generated:**
- Redis configuration
- Connection pooling
- Cache decorators/utilities
- Health checks

**Docker setup:**
```yaml
redis:
  image: redis:7-alpine
  ports:
    - "6379:6379"
  volumes:
    - redis_data:/data
```

**Scale considerations:**
- Micro: Single Redis instance
- Small-Medium: Redis with persistence
- Large: Redis Cluster
- Enterprise: Redis Enterprise

**Cost:** Free (self-hosted), managed from $10/mo

### Option 2: In-Memory

**Best for:**
- Simple applications
- Development/testing
- Single-instance deployments
- Minimal state requirements

**Implementation:**
- Python: Simple dict or LRU cache
- Node.js: Memory cache libraries

**Limitations:**
- Lost on restart
- Not shared across instances
- Limited by RAM

**When to use:**
- Micro scale only
- Stateless applications
- Temporary caching

### Option 3: None

**Best for:**
- Stateless APIs
- Database-backed everything
- Serverless architecture

**Considerations:**
- All state in database
- No caching layer
- Simpler architecture
- May need optimization later

### Comparison

| Aspect | Redis | In-Memory | None |
|--------|-------|-----------|------|
| Performance | Excellent | Good | Database-dependent |
| Scalability | High | Low | Medium |
| Persistence | Optional | None | N/A |
| Complexity | Medium | Low | Very Low |
| Cost | $10+/mo | Free | $0 |

## Phase 7: Background Jobs

**Question:** "Choose background job processing"

### Option 1: BullMQ (Node.js)

**Best for:**
- TypeScript/Node.js backends
- Complex job workflows
- Priority queues
- Scheduled jobs

**Features:**
- Redis-based
- Job priorities
- Delayed jobs
- Retries with backoff
- Rate limiting
- Job events/progress

**Use cases:**
- Email sending
- Report generation
- Data processing
- Image/video processing
- Webhook deliveries

**Generated:**
- Queue configuration
- Worker processes
- Job definitions
- Dashboard integration (Bull Board)

**Example:**
```typescript
import { Queue } from 'bullmq';

const emailQueue = new Queue('emails');

await emailQueue.add('sendWelcome', {
  to: 'user@example.com',
  template: 'welcome'
});
```

### Option 2: Celery (Python)

**Best for:**
- Python backends
- Data processing
- ML/AI workflows
- Complex task chains

**Features:**
- Multiple brokers (Redis, RabbitMQ)
- Task chains/groups
- Scheduled tasks (Celery Beat)
- Result backends
- Monitoring (Flower)

**Use cases:**
- Data ETL
- ML model training
- Report generation
- Batch processing
- Periodic tasks

**Generated:**
- Celery configuration
- Task definitions
- Beat scheduler setup
- Worker startup scripts

**Example:**
```python
from celery import Celery

app = Celery('tasks', broker='redis://localhost')

@app.task
def send_email(to, subject, body):
    # Email sending logic
    pass
```

### Option 3: None

**Best for:**
- Simple APIs
- Synchronous processing
- Serverless architectures
- Minimal complexity

**Alternatives:**
- Process inline
- Use cron jobs
- Cloud functions
- Database triggers

## Phase 8: Real-time Features

**Question:** "Choose real-time communication strategy"

### Option 1: WebSockets

**Best for:**
- Bidirectional communication
- Real-time collaboration
- Live updates
- Chat applications

**Features:**
- Full-duplex communication
- Low latency
- Binary data support
- Custom protocols

**Use cases:**
- Chat/messaging
- Live notifications
- Collaborative editing
- Real-time dashboards
- Multiplayer games

**Generated:**
- WebSocket server setup
- Connection handling
- Authentication
- Message broadcasting
- Client SDK

**Frameworks:**
- FastAPI: WebSocket support built-in
- NestJS: @nestjs/websockets
- Socket.io integration

### Option 2: Server-Sent Events (SSE)

**Best for:**
- One-way server→client updates
- Simple real-time feeds
- Progress updates
- Live logs

**Features:**
- HTTP-based
- Automatic reconnection
- Simpler than WebSockets
- Works with HTTP/2

**Use cases:**
- Live notifications
- Stock tickers
- Progress bars
- Activity feeds
- Log streaming

**Generated:**
- SSE endpoint setup
- Event streaming
- Client connection handling

### Option 3: None

**Best for:**
- Static content
- Polling acceptable
- Simple applications

**Alternatives:**
- Client polling
- Long polling
- Webhooks

## Phase 9: File Storage

**Question:** "Choose file storage solution"

### Option 1: AWS S3

**Best for:**
- Scalable storage
- CDN integration
- Object storage
- Any scale

**Features:**
- Unlimited storage
- S3 Standard, Glacier, etc.
- CloudFront integration
- Presigned URLs
- Lifecycle policies

**Generated:**
- S3 client configuration
- Upload/download utilities
- Presigned URL generation
- Environment variables

**Cost:** ~$0.023/GB/month + transfer

### Option 2: Cloudinary

**Best for:**
- Image/video manipulation
- Automatic optimization
- Media-heavy apps

**Features:**
- Image transformations
- Video transcoding
- CDN included
- Automatic format selection
- Responsive images

**Cost:** Free tier, then ~$99+/month

### Option 3: Supabase Storage

**Best for:**
- Supabase users
- Integrated auth
- Simple storage needs

**Features:**
- PostgreSQL-backed
- RLS integration
- Image transformations
- CDN integration

**Cost:** Included with Supabase plan

### Option 4: Local Filesystem

**Best for:**
- Development
- Small files
- Self-hosted deployments

**Limitations:**
- Not scalable
- No CDN
- Backup challenges

## Phase 10: Email Service

**Question:** "Choose email service provider"

### Option 1: SendGrid

**Features:**
- Transactional email
- Templates
- Analytics
- Good deliverability

**Cost:** Free tier (100 emails/day), then $19.95+/month

### Option 2: AWS SES

**Features:**
- Low cost
- AWS integration
- High volume

**Cost:** $0.10 per 1,000 emails

### Option 3: Resend

**Features:**
- Developer-friendly
- React email templates
- Modern API

**Cost:** Free tier (100 emails/day), then $20+/month

### Option 4: None

Skip email integration

## Phase 11: Monitoring & Observability

### Basic
- Console logging
- Simple error handling

### Sentry
- Error tracking
- Performance monitoring
- Cost: Free tier, then $26+/month

### DataDog
- Full observability
- Metrics, traces, logs
- Cost: $15+/host/month

### OpenTelemetry
- Open standard
- Vendor-neutral
- Self-hosted or managed

## Phase 12: Deployment Strategy

### Vercel + AWS
- Frontend on Vercel
- Backend on AWS

### Docker Full
- Complete containerization
- Deploy anywhere

### Serverless
- AWS Lambda
- Vercel Functions
- Cost-effective for low traffic

## Phase 13: Additional Features

Multi-select:
- [ ] Testing setup (pytest/jest)
- [ ] CI/CD workflows
- [ ] Pre-commit hooks
- [ ] API documentation
- [ ] Feature flags
- [ ] A/B testing
- [ ] Analytics

## Scale-Based Defaults

| Feature | Micro | Small | Medium | Large | Enterprise |
|---------|-------|-------|--------|-------|------------|
| Backend | FastAPI | FastAPI | FastAPI/NestJS | NestJS | Microservices |
| Database | SQLite/Supabase | Supabase/PostgreSQL | PostgreSQL | PostgreSQL Cluster | Distributed |
| State | In-Memory | Redis | Redis | Redis Cluster | Distributed Cache |
| Jobs | None | Optional | BullMQ/Celery | Required | Kafka/RabbitMQ |
| Real-time | None | Optional | SSE | WebSockets | WebSockets + Pub/Sub |
| Storage | Local | S3/Supabase | S3 | S3 + CDN | Multi-region S3 |
| Monitoring | Basic | Sentry | DataDog | DataDog | OpenTelemetry |
| Deploy | Vercel | Vercel/Railway | Docker | Kubernetes | Multi-cloud |

## Decision Trees

### Choosing Backend

```
START
  ↓
Are you a Python shop? → YES → FastAPI
  ↓ NO
Do you need enterprise patterns? → YES → NestJS
  ↓ NO
Is it a simple CRUD app? → YES → API-less (Supabase)
  ↓ NO
FastAPI (recommended)
```

### Choosing Database

```
START
  ↓
Need built-in auth? → YES → Supabase
  ↓ NO
Need full control? → YES → PostgreSQL + JWT
  ↓ NO
Mobile-first? → YES → Firebase
  ↓ NO
PostgreSQL + JWT (recommended)
```

## Project Type Examples

### SaaS Application
- Scale: Small
- Backend: FastAPI
- Frontend: Next.js
- Database: Supabase
- State: Redis
- Jobs: BullMQ
- Real-time: WebSockets
- Storage: S3
- Email: SendGrid
- Monitoring: Sentry
- Deploy: Vercel + AWS

### API Service
- Scale: Micro
- Backend: FastAPI
- Frontend: None
- Database: PostgreSQL
- State: Redis
- Jobs: Celery
- Real-time: None
- Storage: S3
- Email: AWS SES
- Monitoring: Basic
- Deploy: Docker

### E-commerce Platform
- Scale: Medium
- Backend: NestJS
- Frontend: Next.js
- Database: PostgreSQL
- State: Redis
- Jobs: BullMQ
- Real-time: SSE
- Storage: S3 + Cloudinary
- Email: SendGrid
- Monitoring: DataDog
- Deploy: Docker

### Internal Tool
- Scale: Micro
- Backend: FastAPI
- Frontend: Next.js
- Database: PostgreSQL
- State: In-Memory
- Jobs: None
- Real-time: None
- Storage: Local
- Email: None
- Monitoring: Basic
- Deploy: Docker

---

**Author:** Homero Thompson del Lago del Terror
**Last Updated:** 2026-01-19
