# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Nuevo Proyecto Scaffold** is an enterprise-grade project generator with **972 templates**. This repository contains templates and wizard configuration—it is NOT an executable project itself. Projects are generated via Claude Code skill `/nuevo-proyecto`.

**Author:** Homero Thompson del Lago del Terror
**Version:** 6.2.0 (Design Systems + Tauri Desktop Edition)

**Important:** This is a pure template repository. There is no executable code here—the wizard runtime is implemented in the Claude Code skill, not in this repo.

## Repository Structure

```
nuevo-proyecto-scaffold/
├── auth/              # Authentication templates (JWT, OAuth2, Auth0, Clerk, SAML, MFA)
├── backend/           # Backend templates by framework
│   ├── fastapi/       # Python 3.12+ async
│   ├── nestjs/        # TypeScript enterprise
│   ├── go/            # Gin/Echo high-performance
│   ├── rust/          # Actix-web + Rocket
│   ├── graphql/       # Apollo, Strawberry, gqlgen
│   ├── grpc/          # Protobuf definitions
│   └── trpc/          # End-to-end type-safe
├── desktop/           # Desktop application templates
│   └── tauri/         # Tauri v2 (Rust + Web frontend, 74 templates)
├── frontend/          # Frontend templates + Design Systems
│   ├── nextjs/        # Next.js 15 App Router
│   ├── react-spa/     # React + Vite
│   ├── vue/           # Vue 3 Composition API
│   ├── angular/       # Angular 17+ Standalone
│   ├── jquery/        # jQuery + Bootstrap 5
│   └── design-systems/ # 5 UI design systems (86 templates)
├── mobile/            # React Native + Flutter templates
├── edge/              # Cloudflare Workers + Deno Deploy
├── wasm/              # Rust-WASM + AssemblyScript
├── blockchain/        # Web3.js + Ethers.js
├── infrastructure/    # Docker, Kubernetes, Terraform
├── observability/     # OpenTelemetry, Prometheus, Grafana, Jaeger
├── testing/           # Unit, integration, e2e, contract, load, chaos
├── security/          # Rate limiting, encryption, WAF, compliance
├── wizard/            # questions.yaml - wizard configuration
├── _meta/             # manifest.json, versions.json
└── docs/              # ARCHITECTURE.md, TESTING.md, WIZARD-PHASES.md
```

## Key Architecture Concepts

### Template System
- Templates use `.tmpl` extension (e.g., `main.py.tmpl`, `package.json.tmpl`)
- Variable syntax: `{{PROJECT_NAME}}`, `{{AUTHOR}}`, `{{BUNDLE_ID}}`
- Templates are organized by technology stack and feature

### Wizard System (11 Phases)
The wizard in `wizard/questions.yaml` defines 33 questions across phases:
1. **Metadata** - project name, author, license
2. **Scale & Compliance** - MVP → Netflix scale, GDPR/HIPAA/SOC2
3. **Stack** - backend, frontend, mobile, **desktop (Tauri)**, API style, database, cache
4. **Auth** - provider (Supabase/Firebase/Auth0/Clerk/JWT), MFA, RBAC/ABAC
5. **Testing** - unit → chaos engineering (scale-dependent)
6. **Security** - SAST → zero-trust (scale-dependent)
7. **Infrastructure** - Vercel → Kubernetes, IaC
8. **Observability** - logging → SLO monitoring
9. **Developer Experience** - DevContainers, Storybook, MSW, **Design System**
10. **Advanced** - Edge computing, WASM, Blockchain
11. **Extras** - email, storage, AI/LLM, payments

### Scale Tiers (affects defaults)
| Scale | Team | Users | Testing | Security | Deploy |
|-------|------|-------|---------|----------|--------|
| MVP | 1-2 | <1K | unit+integration | SAST+secrets | Vercel |
| Scale-up | 3-10 | 1K-100K | +e2e+contract | +SCA+container | Docker |
| Enterprise | 10-50 | 100K-1M | +load+visual | +DAST+SBOM+WAF | K8s |
| Netflix | 50+ | 1M+ | +chaos+mutation | +runtime+zero-trust | Multi-region K8s |

## Usage

This scaffold is invoked via Claude Code, not directly:

```bash
# Inside Claude Code
/nuevo-proyecto
```

The wizard guides through all 11 phases to generate a complete project.

## Contributing to Templates

Since this is a pure template repository, there is no build or test system. Contributions involve editing `.tmpl` files and `wizard/questions.yaml`.

### Validation Commands

```bash
# Validate YAML syntax
python -c "import yaml; yaml.safe_load(open('wizard/questions.yaml'))"

# Find all templates by category
find backend/ -name "*.tmpl" | wc -l
find frontend/ -name "*.tmpl" | wc -l
find desktop/ -name "*.tmpl" | wc -l

# Check template variable usage
grep -r "{{PROJECT_NAME}}" --include="*.tmpl" | head -20
```

## Adding New Templates

1. Create template files in appropriate directory with `.tmpl` extension
2. Use `{{VARIABLE_NAME}}` syntax for template variables
3. Update wizard options in `wizard/questions.yaml` if adding new framework/feature
4. Update `_meta/manifest.json` with new template count
5. Update `docs/WIZARD-PHASES.md` with new options

## Code Style (for templates)

- **Python templates**: PEP 8 style, 100 char line limit
- **TypeScript templates**: ESLint/Prettier compatible
- **Commits**: Conventional Commits format (`feat(wizard): add scale selection`)

### Author Attribution in Templates

Templates for new files should include author placeholder:

```python
# For Python templates
"""
{{PROJECT_NAME}} - Brief description

Autor: {{AUTHOR}}
"""
```

## Generated Project Structure

When `/nuevo-proyecto` runs, it creates a Turborepo monorepo:

```
generated-project/
├── apps/
│   ├── web/           # Frontend (Next.js/React/Vue/Angular/jQuery)
│   ├── api/           # Backend (FastAPI/NestJS/Go/Rust)
│   ├── mobile/        # Optional (React Native/Flutter)
│   └── desktop/       # Optional (Tauri desktop app)
├── packages/
│   ├── ui/            # Shared UI components
│   ├── config/        # Shared configs
│   ├── types/         # Shared TypeScript types
│   ├── database/      # DB schemas & migrations
│   └── utils/         # Shared utilities
├── infrastructure/    # Docker, K8s, Terraform
└── .github/workflows/ # CI/CD pipelines
```

## Tauri Desktop Templates

When desktop platform is selected as `tauri`, `tauri-react`, or `tauri-vue`:

```
desktop/tauri/
├── src-tauri/                    # Rust backend
│   ├── Cargo.toml.tmpl          # Dependencies (Tauri v2, sqlx, argon2)
│   ├── tauri.conf.json.tmpl     # App config, capabilities, bundle settings
│   ├── build.rs.tmpl            # Build script
│   ├── capabilities/            # Security permissions (Tauri v2)
│   └── src/
│       ├── main.rs.tmpl         # Entry point with plugin setup
│       ├── lib.rs.tmpl          # Library exports
│       ├── config.rs.tmpl       # App configuration
│       ├── error.rs.tmpl        # Custom error types
│       ├── commands/            # IPC handlers (app, auth, files, system)
│       ├── state/               # Thread-safe app state
│       ├── db/                  # SQLite with sqlx
│       ├── models/              # User, Settings models
│       └── services/            # Auth, Settings services
├── src/lib/                     # Frontend integration
│   ├── tauri.ts.tmpl           # Type-safe Tauri API wrapper
│   ├── tauri-store.ts.tmpl     # Persistent store wrapper
│   ├── hooks/useTauri.ts.tmpl  # React hooks
│   └── composables/useTauri.ts.tmpl  # Vue composables
├── tests/
│   ├── rust/                    # Cargo tests
│   └── e2e/                     # Playwright E2E tests
├── Dockerfile.tmpl              # Multi-stage build
├── docker-compose.yml.tmpl      # Dev environment
└── .github/workflows/tauri-release.yml.tmpl  # Multi-platform CI/CD
```

### Tauri Template Variables
- `{{PROJECT_NAME}}` - kebab-case identifier
- `{{AUTHOR}}` - Author name
- `{{APP_TITLE}}` - Window title
- `{{BUNDLE_ID}}` - Bundle identifier (e.g., `com.example.app`)

## Design Systems (86 templates)

When `design_system` is selected in wizard Phase 9:

```
frontend/design-systems/
├── README.md                 # Central documentation
├── COMPARISON.md             # Detailed comparison guide
├── index.ts.tmpl            # Design system selector
├── design-tokens.ts.tmpl    # Shared design tokens
├── preview-data.json.tmpl   # Wizard preview data
│
├── geist/                   # Vercel/Geist aesthetic (12 templates)
│   ├── tailwind.config.geist.js.tmpl
│   ├── globals.css.tmpl
│   ├── components/react/    # Button, Card, Input, Modal, Navbar
│   ├── components/vue/      # Button, Card, Input
│   └── examples/LandingPage.tsx.tmpl
│
├── glassmorphism/           # Frosted glass effect (13 templates)
│   ├── tailwind.config.glass.js.tmpl
│   ├── globals.css.tmpl
│   ├── components/react/    # GlassButton, GlassCard, GlassModal, etc.
│   ├── components/vue/
│   └── examples/LandingPage.tsx.tmpl
│
├── neumorphism/             # Soft UI (15 templates)
│   ├── tailwind.config.neu.js.tmpl
│   ├── globals.css.tmpl
│   ├── components/react/    # NeuButton, NeuCard, NeuToggle, NeuSlider, etc.
│   ├── components/vue/
│   └── examples/LandingPage.tsx.tmpl
│
├── claymorphism/            # Clay/plasticine effect (15 templates)
│   ├── tailwind.config.clay.js.tmpl
│   ├── globals.css.tmpl
│   ├── components/react/    # ClayButton, ClayCard, ClayAvatar, etc.
│   ├── components/vue/
│   └── examples/LandingPage.tsx.tmpl
│
├── neo-brutalism/           # Bold & chunky (16 templates)
│   ├── tailwind.config.brutal.js.tmpl
│   ├── globals.css.tmpl
│   ├── components/react/    # BrutalButton, BrutalHeading, BrutalMarquee, etc.
│   ├── components/vue/
│   └── examples/LandingPage.tsx.tmpl
│
└── shared/                  # Shared utilities (10 templates)
    ├── hooks/               # useDesignSystem, useTheme (React)
    ├── composables/         # useDesignSystem, useTheme (Vue)
    ├── context/             # DesignSystemProvider
    ├── utils/               # cn(), animations
    └── types/               # TypeScript interfaces
```

### Design System Options

| System | Style | Best For | Accessibility |
|--------|-------|----------|---------------|
| **Geist** | Sharp minimalism, gradients | Tech products, SaaS | ⭐⭐⭐⭐⭐ |
| **Glassmorphism** | Frosted glass, blur | Futuristic apps, dark mode | ⭐⭐⭐⭐ |
| **Neumorphism** | Soft shadows, tactile | Calm UIs, settings | ⭐⭐⭐ |
| **Claymorphism** | Organic shapes, playful | Consumer apps, kids | ⭐⭐⭐⭐ |
| **Neo-Brutalism** | Bold colors, chunky | Creative agencies, portfolios | ⭐⭐⭐⭐⭐ |

## Key Files Reference

| File | Purpose |
|------|---------|
| `wizard/questions.yaml` | All wizard phases and questions |
| `_meta/manifest.json` | Project metadata |
| `docs/ARCHITECTURE.md` | Monorepo architecture patterns |
| `docs/TESTING.md` | Testing strategy by scale |
| `docs/WIZARD-PHASES.md` | Detailed phase documentation |
