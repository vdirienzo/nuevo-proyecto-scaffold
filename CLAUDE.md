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
├── backend/           # Backend templates (fastapi, nestjs, go, rust, graphql, grpc, trpc)
├── desktop/           # Desktop templates (tauri/)
├── frontend/          # Frontend templates + design-systems/
├── mobile/            # React Native + Flutter templates
├── edge/              # Cloudflare Workers + Deno Deploy
├── wasm/              # Rust-WASM + AssemblyScript
├── blockchain/        # Web3.js + Ethers.js
├── infrastructure/    # Docker, Kubernetes, Terraform
├── observability/     # OpenTelemetry, Prometheus, Grafana, Jaeger
├── testing/           # Unit, integration, e2e, contract, load, chaos
├── security/          # Rate limiting, encryption, WAF, compliance
├── wizard/            # questions.yaml - wizard configuration (11 phases, 33 questions)
├── _meta/             # manifest.json, versions.json
└── docs/              # ARCHITECTURE.md, TESTING.md, WIZARD-PHASES.md
```

## Template System

### File Format
- Templates use `.tmpl` extension (e.g., `main.py.tmpl`, `package.json.tmpl`)
- Variable syntax: `{{VARIABLE_NAME}}`
- Templates organized by technology stack and feature

### Template Variables

Variables available in all templates (from wizard `questions.yaml`):

| Variable | Source | Example |
|----------|--------|---------|
| `{{PROJECT_NAME}}` | Phase 1: project_name | `my-awesome-app` |
| `{{PROJECT_NAME_SNAKE}}` | Derived | `my_awesome_app` |
| `{{PROJECT_DESCRIPTION}}` | Phase 1: project_description | `A modern web app` |
| `{{AUTHOR}}` | Phase 1: project_author | `Homero Thompson del Lago del Terror` |
| `{{LICENSE}}` | Phase 1: project_license | `mit`, `apache-2.0`, `proprietary` |
| `{{REPO_URL}}` | Phase 1: repo_url | `https://github.com/user/repo` |

Tauri-specific variables (desktop templates):

| Variable | Description |
|----------|-------------|
| `{{APP_TITLE}}` | Window title for desktop app |
| `{{BUNDLE_ID}}` | Bundle identifier (e.g., `com.example.app`) |

## Wizard Phases (11 total)

The wizard in `wizard/questions.yaml` defines 33 questions:

1. **Metadata** - project_name, project_description, project_author, project_license, repo_url
2. **Scale & Compliance** - scale_level (mvp/scale-up/enterprise/netflix), compliance_requirements, data_sensitivity
3. **Stack** - backend_framework, frontend_framework, mobile_platform, desktop_platform, api_style, database, cache_layer
4. **Auth** - auth_provider, auth_methods, authorization_model
5. **Testing** - testing_level, coverage_target
6. **Security** - security_level, encryption
7. **Infrastructure** - deploy_target, iac_tool, ci_cd
8. **Observability** - observability_stack, alerting
9. **Developer Experience** - dev_tools, code_quality, design_system
10. **Advanced** - edge_computing, wasm_modules, blockchain_integration
11. **Extras** - additional_features (email, storage, queues, ai-integration, etc.)

### Scale Tiers (affects wizard defaults)

| Scale | Team | Users | Defaults |
|-------|------|-------|----------|
| MVP | 1-2 | <1K | unit+integration, SAST+secrets, Vercel |
| Scale-up | 3-10 | 1K-100K | +e2e+contract, +SCA+container, Docker |
| Enterprise | 10-50 | 100K-1M | +load+visual, +DAST+SBOM+WAF, K8s |
| Hyperscale | 50+ | 1M+ | +chaos+mutation, +runtime+zero-trust, Multi-region K8s |

## Validation Commands

```bash
# Validate wizard YAML syntax
python -c "import yaml; yaml.safe_load(open('wizard/questions.yaml'))"

# Count templates by category
find backend/ -name "*.tmpl" | wc -l
find frontend/ -name "*.tmpl" | wc -l
find desktop/ -name "*.tmpl" | wc -l

# Check template variable usage
grep -r "{{PROJECT_NAME}}" --include="*.tmpl" | head -20

# Find all unique template variables used
grep -rhoE '\{\{[A-Z_]+\}\}' --include="*.tmpl" | sort -u

# Validate no syntax errors in templates (basic check)
find . -name "*.tmpl" -exec grep -l "{{[^}]*$" {} \;
```

## Adding New Templates

1. Create template files in appropriate directory with `.tmpl` extension
2. Use `{{VARIABLE_NAME}}` syntax for template variables
3. Update wizard options in `wizard/questions.yaml` if adding new framework/feature
4. Update `_meta/manifest.json` with new template count
5. Update `docs/WIZARD-PHASES.md` with new options

### Author Attribution in Templates

Templates for new files should include author placeholder:

```python
# For Python templates
"""
{{PROJECT_NAME}} - Brief description

Autor: {{AUTHOR}}
"""
```

```typescript
// For TypeScript templates
/**
 * @fileoverview Brief description
 * @author {{AUTHOR}}
 */
```

## Code Style (for templates)

- **Python templates**: PEP 8 style, 100 char line limit
- **TypeScript templates**: ESLint/Prettier compatible
- **Commits**: Conventional Commits format (`feat(wizard): add scale selection`)

Commit scopes: `wizard`, `templates`, `cli`, `config`, `docs`, `deps`

## Key Files Reference

| File | Purpose |
|------|---------|
| `wizard/questions.yaml` | All wizard phases and questions with scale-based defaults |
| `_meta/manifest.json` | Project metadata and template counts |
| `docs/ARCHITECTURE.md` | Monorepo architecture patterns |
| `docs/TESTING.md` | Testing strategy by scale |
| `docs/WIZARD-PHASES.md` | Detailed phase documentation |
| `CONTRIBUTING.md` | Contribution guidelines with dev setup |

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

## Design Systems (86 templates)

Available in `frontend/design-systems/`:

| System | Style | Templates |
|--------|-------|-----------|
| `geist` | Vercel aesthetic, sharp minimalism | 12 |
| `glassmorphism` | Frosted glass, backdrop-blur | 13 |
| `neumorphism` | Soft shadows, tactile feel | 15 |
| `claymorphism` | Organic shapes, pastel colors | 15 |
| `neo-brutalism` | Bold colors, chunky typography | 16 |
| `shared` | React hooks, Vue composables, tokens | 10 |

Each includes Tailwind config, React components, Vue components, and landing page example.

## Desktop (Tauri) Templates (74 templates)

Located in `desktop/tauri/`:
- `src-tauri/` - Rust backend (Cargo, commands, db, models, services)
- `src/lib/` - Frontend integration (tauri.ts, hooks, composables)
- `tests/` - Rust tests + Playwright E2E
- CI/CD for multi-platform builds (Windows, macOS, Linux)
