# Usage Guide

Complete guide for using Nuevo Proyecto Scaffold to generate production-ready fullstack applications.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Running the Wizard](#running-the-wizard)
- [Wizard Phases Overview](#wizard-phases-overview)
- [Customizing Outputs](#customizing-outputs)
- [Post-Generation Steps](#post-generation-steps)
- [Common Workflows](#common-workflows)
- [Advanced Usage](#advanced-usage)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)

## Prerequisites

### Required

- **Python 3.12 or higher**
  ```bash
  python --version  # Should show 3.12.x or higher
  ```

- **uv package manager** (recommended) or pip
  ```bash
  # Install uv
  curl -LsSf https://astral.sh/uv/install.sh | sh

  # Verify installation
  uv --version
  ```

- **Git**
  ```bash
  git --version
  ```

### Optional (based on project choices)

- **Docker Desktop**: For containerized development
- **Node.js 18+**: For frontend development
- **PostgreSQL**: If using local database (or use Docker)

## Installation

### Option 1: Via uv (Recommended)

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/nuevo-proyecto-scaffold.git
cd nuevo-proyecto-scaffold

# Install dependencies
uv sync

# Verify installation
uv run python -m nuevo_proyecto_scaffold.wizard --help
```

### Option 2: Via pip

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/nuevo-proyecto-scaffold.git
cd nuevo-proyecto-scaffold

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -e .

# Verify installation
python -m nuevo_proyecto_scaffold.wizard --help
```

### Option 3: Global Installation (Future)

```bash
# Once published to PyPI
uv tool install nuevo-proyecto-scaffold

# Run from anywhere
nuevo-proyecto --help
```

## Quick Start

Generate a project in 5 minutes:

```bash
# Navigate to your projects directory
cd ~/projects

# Run the wizard
uv run python -m nuevo_proyecto_scaffold.wizard

# Follow the prompts:
# 1. Project name: my-awesome-app
# 2. Scale: Small (2-5 developers, moderate traffic)
# 3. Backend: FastAPI
# 4. Frontend: Next.js
# 5. Database: Supabase
# ... (continue through wizard)

# Navigate to generated project
cd my-awesome-app

# Start development
docker compose up -d
```

Your project is ready! Visit:
- Frontend: http://localhost:3000
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs

## Running the Wizard

### Interactive Mode (Default)

```bash
uv run python -m nuevo_proyecto_scaffold.wizard
```

The wizard will guide you through 13 phases:

1. **Project Name**: Enter your project name
2. **Scale Selection**: Choose project scale (micro to enterprise)
3. **Backend Framework**: FastAPI, NestJS, or API-less
4. **Frontend Framework**: Next.js (more options coming)
5. **Database & Auth**: Supabase, PostgreSQL+JWT, Firebase
6. **State Management**: Redis, In-Memory, or None
7. **Background Jobs**: BullMQ, Celery, or None
8. **Real-time Features**: WebSockets, Server-Sent Events, or None
9. **File Storage**: S3, Cloudinary, Supabase Storage, or Local
10. **Email Service**: SendGrid, AWS SES, Resend, or None
11. **Monitoring**: Sentry, DataDog, OpenTelemetry, or Basic
12. **Deployment**: Vercel+AWS, Docker, Serverless
13. **Additional Features**: Testing, CI/CD, Documentation

### Non-Interactive Mode (Future)

```bash
# Using config file
uv run python -m nuevo_proyecto_scaffold.wizard --config project-config.yaml

# Using command-line flags
uv run python -m nuevo_proyecto_scaffold.wizard \
  --name my-api \
  --scale small \
  --backend fastapi \
  --database postgresql \
  --deploy docker
```

## Wizard Phases Overview

### Phase 1: Project Name

**What it asks:** Enter your project name

**Guidelines:**
- Use lowercase letters, numbers, and hyphens
- Valid: `my-app`, `api-v2`, `ecommerce-backend`
- Invalid: `My App`, `api_v2`, `ECOMMERCE`
- Will be used for:
  - Directory name
  - Package name (converted to snake_case)
  - Git repository name
  - Docker image names

**Example:**
```
Project name: my-saas-app
✓ Valid format
✓ Directory will be created at: ~/projects/my-saas-app
```

### Phase 2: Scale Selection

**What it asks:** Choose your project scale

**Options:**
| Scale | Team Size | Traffic | Use Case |
|-------|-----------|---------|----------|
| Micro | 1 developer | < 1K users | MVPs, experiments |
| Small | 2-5 developers | < 50K users | Startups, internal tools |
| Medium | 5-20 developers | < 500K users | Growing products |
| Large | 20-50 developers | < 5M users | Established products |
| Enterprise | 50+ developers | 5M+ users | Large organizations |

**Impact:**
- Affects default selections in later phases
- Influences architecture patterns
- Determines monitoring/scaling features
- Adjusts CI/CD complexity

**Recommendation System:**
The wizard provides recommendations based on:
- Team size
- Expected traffic
- Complexity requirements
- Budget considerations

### Phase 3-13: Detailed Configuration

See [WIZARD-PHASES.md](./WIZARD-PHASES.md) for detailed explanation of each phase.

## Customizing Outputs

### During Generation

**Selecting Features:**
- Use arrow keys to navigate options
- Press Space to select/deselect (multi-select questions)
- Press Enter to confirm
- Type 'back' to return to previous phase

**Modifying Defaults:**
- The wizard suggests defaults based on scale
- You can override any suggestion
- Yellow indicators show recommended options

### After Generation

**Modifying Configuration:**

```bash
# Edit environment variables
cd my-project
cp .env.example .env
nano .env  # or use your preferred editor
```

**Customizing Templates:**

1. Locate the generated file you want to modify
2. Make your changes
3. Test thoroughly
4. Consider contributing improvements back

**Extending Functionality:**

```bash
# Add new dependencies
cd my-project
uv add package-name

# Or for Node.js projects
cd apps/web
npm install package-name
```

## Post-Generation Steps

### 1. Review Generated Files

```bash
cd my-project
tree -L 2  # View structure

# Key files to review:
cat README.md              # Project-specific documentation
cat .env.example           # Environment variables template
cat docker-compose.yml     # Local development setup
```

### 2. Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit with your values
nano .env
```

**Required variables (examples):**
```env
# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname

# Auth
JWT_SECRET=your-secret-key-here

# Optional: External services
SMTP_HOST=smtp.sendgrid.net
AWS_ACCESS_KEY_ID=your-key
SENTRY_DSN=your-sentry-dsn
```

### 3. Start Development Environment

**With Docker (Recommended):**
```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

**Without Docker:**

```bash
# Backend (FastAPI example)
cd backend
uv sync
uv run uvicorn src.main:app --reload

# Frontend (Next.js)
cd apps/web
npm install
npm run dev
```

### 4. Run Database Migrations

**Alembic (FastAPI + PostgreSQL):**
```bash
cd backend
uv run alembic upgrade head
```

**Drizzle (NestJS):**
```bash
cd backend
npm run db:push
```

**Supabase:**
```bash
# Migrations are handled via Supabase dashboard
# Or use Supabase CLI
supabase db push
```

### 5. Initialize Git Repository

```bash
# Repository is already initialized
git status

# Make initial commit if needed
git add .
git commit -m "feat: initial project setup from scaffold"

# Connect to remote
git remote add origin https://github.com/username/my-project.git
git push -u origin main
```

### 6. Run Tests

```bash
# Backend tests
cd backend
uv run pytest -v

# Frontend tests
cd apps/web
npm test

# Run all tests via Turborepo
npm test
```

### 7. Verify CI/CD

```bash
# Push to GitHub to trigger workflows
git push

# Check GitHub Actions tab for:
# ✓ Tests workflow
# ✓ Lint workflow
# ✓ Build workflow
# ✓ Security scan workflow
```

## Common Workflows

### Workflow 1: SaaS Application

**Configuration:**
- Scale: Small
- Backend: FastAPI
- Frontend: Next.js
- Database: Supabase (includes auth)
- State: Redis
- Jobs: BullMQ
- Real-time: WebSockets
- Storage: Supabase Storage
- Email: SendGrid
- Monitoring: Sentry
- Deploy: Vercel + AWS

**Post-generation:**
1. Configure Supabase project
2. Add SendGrid API key
3. Set up Vercel project
4. Configure AWS credentials
5. Deploy!

### Workflow 2: API Service (Backend Only)

**Configuration:**
- Scale: Micro
- Backend: FastAPI
- Frontend: None (API-only)
- Database: PostgreSQL
- State: None
- Jobs: Celery
- Real-time: None
- Storage: S3
- Email: AWS SES
- Monitoring: Basic
- Deploy: Docker

**Post-generation:**
1. Start PostgreSQL via Docker
2. Run migrations
3. Test endpoints with curl/Postman
4. Build Docker image
5. Deploy to your infrastructure

### Workflow 3: E-commerce Platform

**Configuration:**
- Scale: Medium
- Backend: NestJS
- Frontend: Next.js
- Database: PostgreSQL + Drizzle
- State: Redis
- Jobs: BullMQ
- Real-time: Server-Sent Events
- Storage: S3
- Email: SendGrid
- Monitoring: DataDog
- Deploy: Docker full

**Post-generation:**
1. Configure Stripe integration (manual)
2. Set up payment webhooks
3. Configure product catalog
4. Test checkout flow
5. Set up staging environment

### Workflow 4: Internal Tool

**Configuration:**
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

**Post-generation:**
1. Minimal setup required
2. Run locally with docker-compose
3. No external dependencies
4. Perfect for rapid development

## Advanced Usage

### Custom Templates

Create project-specific template overrides:

```bash
# Create custom template directory
mkdir -p ~/.nuevo-proyecto/templates/custom

# Add your template
cat > ~/.nuevo-proyecto/templates/custom/my-file.py.j2 << EOF
"""Custom template file."""
# Your custom content here
EOF

# Use custom template path
uv run python -m nuevo_proyecto_scaffold.wizard \
  --template-path ~/.nuevo-proyecto/templates/custom
```

### Scripting Project Generation

```python
from nuevo_proyecto_scaffold.wizard import ProjectConfig
from nuevo_proyecto_scaffold.generator import generate_project
from pathlib import Path

# Define configuration
config = ProjectConfig(
    backend="fastapi",
    frontend="nextjs",
    database="postgresql",
    scale="small",
    # ... other options
)

# Generate project
result = generate_project(
    project_name="auto-generated-app",
    config=config,
    output_path=Path("~/projects")
)

if result:
    print("✓ Project generated successfully")
```

### Batch Generation

```bash
# Generate multiple projects from configs
for config in configs/*.yaml; do
  uv run python -m nuevo_proyecto_scaffold.wizard --config "$config"
done
```

## Troubleshooting

### Issue: "Command not found: uv"

**Solution:**
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Restart terminal or reload shell
source ~/.bashrc  # or ~/.zshrc
```

### Issue: "Python version 3.12+ required"

**Solution:**
```bash
# Install Python 3.12
# On Ubuntu/Debian:
sudo apt install python3.12

# On macOS:
brew install python@3.12

# On Windows:
# Download from python.org
```

### Issue: Docker containers won't start

**Solution:**
```bash
# Check Docker is running
docker info

# Check ports are not in use
lsof -i :3000
lsof -i :8000

# Remove old containers
docker compose down -v
docker compose up -d
```

### Issue: Database connection failed

**Solution:**
```bash
# Check DATABASE_URL in .env
cat .env | grep DATABASE_URL

# Verify PostgreSQL is running
docker compose ps

# Check logs
docker compose logs db

# Reset database
docker compose down -v
docker compose up -d db
```

### Issue: TypeScript errors in frontend

**Solution:**
```bash
# Reinstall dependencies
cd apps/web
rm -rf node_modules package-lock.json
npm install

# Clear Next.js cache
rm -rf .next
npm run dev
```

### Issue: Tests failing after generation

**Solution:**
```bash
# Backend tests
cd backend
uv sync  # Ensure dependencies installed
uv run pytest -v --tb=short  # Show detailed errors

# Frontend tests
cd apps/web
npm install
npm test -- --verbose
```

### Issue: CI/CD workflow failing

**Common causes:**
1. Missing secrets in GitHub repository settings
2. Outdated GitHub Actions versions
3. Test failures not caught locally

**Solution:**
```bash
# Test locally first
uv run pytest -v
npm test

# Check GitHub Actions logs
# Go to: Repository → Actions → Click failed workflow

# Update secrets in GitHub:
# Repository → Settings → Secrets and variables → Actions
```

### Issue: Generated project structure looks wrong

**Solution:**
```bash
# Re-generate with verbose mode
uv run python -m nuevo_proyecto_scaffold.wizard --verbose

# Check wizard version
uv run python -m nuevo_proyecto_scaffold.wizard --version

# Update to latest
cd nuevo-proyecto-scaffold
git pull
uv sync
```

## FAQ

### Q: Can I modify the generated project?

**A:** Yes! The generated project is yours to modify. The scaffold provides a starting point with best practices, but you should customize it to your needs.

### Q: Do I need to use Docker?

**A:** No, but it's recommended for consistency. You can run services directly:
- FastAPI: `uvicorn src.main:app --reload`
- Next.js: `npm run dev`
- PostgreSQL: Install locally or use managed service

### Q: Can I add more features after generation?

**A:** Yes. You can:
1. Manually add features by following patterns in generated code
2. Re-run the wizard in a different directory and copy relevant parts
3. Use the wizard as a learning tool for best practices

### Q: How do I upgrade to newer template versions?

**A:** Currently, manual upgrade by:
1. Generate new project with updated wizard
2. Compare with your existing project
3. Cherry-pick improvements

Future: Migration tools and upgrade guides.

### Q: What if I choose the wrong scale?

**A:** You can:
1. Start smaller and scale up (easier)
2. Re-generate with different scale
3. Manually adjust complexity

Most architectural decisions work across scales.

### Q: Can I use this for commercial projects?

**A:** Yes! The scaffold is MIT licensed. Generated code is yours without restrictions.

### Q: Do you support other frameworks?

**A:** Currently: FastAPI (Python), NestJS (TypeScript), Next.js (React)

Coming soon:
- Django (Python)
- Express (TypeScript)
- SvelteKit (Frontend)
- Flutter (Mobile)

### Q: How do I contribute templates?

**A:** See [CONTRIBUTING.md](../CONTRIBUTING.md) for detailed instructions on adding new templates.

### Q: Is there support for monorepo?

**A:** Yes! When selecting both frontend and backend, a Turborepo monorepo is generated with:
- Shared packages
- Unified scripts
- Coordinated CI/CD

### Q: What about mobile apps?

**A:** Currently focused on web. Mobile support (React Native, Flutter) is planned for future releases.

### Q: Can I use this with existing projects?

**A:** The wizard is designed for new projects. For existing projects:
1. Generate a new project with similar stack
2. Compare configurations
3. Selectively adopt patterns and tools

### Q: Where can I get help?

**A:**
- Check this documentation
- Search existing GitHub issues
- Open a new issue with details
- Join discussions on GitHub

---

## Next Steps

- **Detailed wizard documentation**: See [WIZARD-PHASES.md](./WIZARD-PHASES.md)
- **Contributing**: See [CONTRIBUTING.md](../CONTRIBUTING.md)
- **Examples**: Browse `examples/` directory (coming soon)

---

**Author:** Homero Thompson del Lago del Terror
**Last Updated:** 2026-01-19
