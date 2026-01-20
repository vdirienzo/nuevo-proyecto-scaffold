# Deno Deploy Templates - Index

Complete template collection for edge computing applications with Deno Deploy.

## Quick Navigation

### Documentation
- [README.md](README.md) - Complete setup and deployment guide
- [EXAMPLES.md](EXAMPLES.md) - Practical code examples for common use cases
- [INDEX.md](INDEX.md) - This file

### Configuration Files
- [deno.json.tmpl](deno.json.tmpl) - Deno project configuration
- [deno.lock.tmpl](deno.lock.tmpl) - Dependency lock file
- [import_map.json.tmpl](import_map.json.tmpl) - Import aliases
- [fresh.config.ts.tmpl](fresh.config.ts.tmpl) - Fresh framework config
- [.env.example.tmpl](.env.example.tmpl) - Environment variables template
- [.gitignore.tmpl](.gitignore.tmpl) - Git ignore rules

### Entry Points
- [main.ts.tmpl](main.ts.tmpl) - Main application entry point
- [dev.ts.tmpl](dev.ts.tmpl) - Fresh development entry point

### Core Application
- [src/app.ts.tmpl](src/app.ts.tmpl) - Oak application setup with middleware
- [src/router.ts.tmpl](src/router.ts.tmpl) - URL routing configuration

### Request Handlers
- [src/handlers/api.ts.tmpl](src/handlers/api.ts.tmpl) - CRUD API endpoints
- [src/handlers/auth.ts.tmpl](src/handlers/auth.ts.tmpl) - JWT authentication handlers
- [src/handlers/static.ts.tmpl](src/handlers/static.ts.tmpl) - Static file serving
- [src/handlers/websocket.ts.tmpl](src/handlers/websocket.ts.tmpl) - WebSocket connections
- [src/handlers/hono-app.ts.tmpl](src/handlers/hono-app.ts.tmpl) - Alternative Hono framework setup

### Middleware
- [src/middleware/cors.ts.tmpl](src/middleware/cors.ts.tmpl) - CORS configuration
- [src/middleware/auth.ts.tmpl](src/middleware/auth.ts.tmpl) - JWT authentication middleware
- [src/middleware/logger.ts.tmpl](src/middleware/logger.ts.tmpl) - Request logging
- [src/middleware/error-handler.ts.tmpl](src/middleware/error-handler.ts.tmpl) - Global error handling

### Services
- [src/services/kv.ts.tmpl](src/services/kv.ts.tmpl) - Deno KV wrapper service
- [src/services/database.ts.tmpl](src/services/database.ts.tmpl) - PostgreSQL/Supabase connection
- [src/services/cache.ts.tmpl](src/services/cache.ts.tmpl) - Caching service with TTL

### Utilities
- [src/utils/response.ts.tmpl](src/utils/response.ts.tmpl) - API response helpers
- [src/utils/validation.ts.tmpl](src/utils/validation.ts.tmpl) - Request validation with Zod
- [src/utils/crypto.ts.tmpl](src/utils/crypto.ts.tmpl) - Password hashing and crypto utilities

### Type Definitions
- [src/types/index.ts.tmpl](src/types/index.ts.tmpl) - TypeScript type definitions

### Fresh Framework

#### Islands (Interactive Components)
- [islands/Counter.tsx.tmpl](islands/Counter.tsx.tmpl) - Example counter island
- [islands/AuthForm.tsx.tmpl](islands/AuthForm.tsx.tmpl) - Authentication form island

#### Routes (Server-Side Rendered)
- [routes/index.tsx.tmpl](routes/index.tsx.tmpl) - Home page
- [routes/login.tsx.tmpl](routes/login.tsx.tmpl) - Login page
- [routes/register.tsx.tmpl](routes/register.tsx.tmpl) - Registration page
- [routes/_middleware.ts.tmpl](routes/_middleware.ts.tmpl) - Fresh middleware
- [routes/api/[...path].ts.tmpl](routes/api/[...path].ts.tmpl) - API catch-all route

### Static Assets
- [static/styles.css.tmpl](static/styles.css.tmpl) - Global CSS styles

### Testing
- [tests/app_test.ts.tmpl](tests/app_test.ts.tmpl) - Application tests
- [tests/handlers_test.ts.tmpl](tests/handlers_test.ts.tmpl) - Handler unit tests

### Database Scripts
- [scripts/migrate.ts.tmpl](scripts/migrate.ts.tmpl) - Database migration runner
- [scripts/seed.ts.tmpl](scripts/seed.ts.tmpl) - Database seeding script

### CI/CD
- [.github/workflows/deploy.yml.tmpl](.github/workflows/deploy.yml.tmpl) - Deployment workflow
- [.github/workflows/test.yml.tmpl](.github/workflows/test.yml.tmpl) - Testing workflow

### IDE Configuration
- [.vscode/settings.json.tmpl](.vscode/settings.json.tmpl) - VS Code settings
- [.vscode/extensions.json.tmpl](.vscode/extensions.json.tmpl) - Recommended extensions

## Template Categories

### By Feature

**Authentication & Security**
- JWT authentication (auth.ts handlers + middleware)
- Password hashing (crypto.ts utilities)
- CORS configuration (cors.ts middleware)
- Request validation (validation.ts utilities)

**Data Management**
- Deno KV integration (kv.ts service)
- PostgreSQL/Supabase (database.ts service)
- Caching layer (cache.ts service)
- Database migrations (migrate.ts script)

**Real-time Features**
- WebSocket connections (websocket.ts handler)
- BroadcastChannel messaging
- Server-sent events ready

**Frontend**
- Fresh islands architecture
- Server-side rendering
- Pre-rendered static pages
- Interactive components with signals

**Developer Experience**
- Hot reload development mode
- Type checking with TypeScript
- Testing with Deno test
- CI/CD with GitHub Actions
- VS Code integration

## Tech Stack

### Runtime & Framework
- **Deno 2.x** - Secure runtime for JavaScript and TypeScript
- **Oak 16.0+** - Middleware framework for Deno
- **Fresh 1.6+** - Full-stack web framework with islands
- **Hono 4.0+** - Alternative ultra-fast web framework

### Frontend
- **Preact** - Lightweight React alternative
- **Preact Signals** - Reactive state management
- **Fresh Islands** - Partial hydration architecture

### Storage & Database
- **Deno KV** - Built-in key-value store
- **PostgreSQL** - Relational database
- **Supabase** - Backend-as-a-Service

### Authentication
- **djwt** - JWT for Deno
- **PBKDF2** - Password hashing

### Validation
- **Zod** - TypeScript-first schema validation

### Development
- **Deno Standard Library** - Official utilities (jsr:@std/*)
- **Deno Test** - Built-in testing framework
- **Deno Fmt** - Code formatter
- **Deno Lint** - Linter

## Usage

### For Template Users

1. Copy the entire directory structure
2. Replace placeholders:
   - `{{PROJECT_NAME}}` with your project name
   - `{{AUTHOR}}` with your name
3. Remove `.tmpl` extensions from all files
4. Run `deno task dev` to start development

### For Scaffold Integration

These templates are designed for integration with the `/nuevo-proyecto` wizard:

```typescript
// Example integration
const templates = await loadTemplates('/edge/deno-deploy/');
const rendered = renderTemplates(templates, {
  PROJECT_NAME: userInput.projectName,
  AUTHOR: userInput.author || 'Homero Thompson del Lago del Terror'
});
await writeProject(outputDir, rendered);
```

## File Count

- **Total Files**: 44
- **Code Templates**: 32
- **Configuration**: 7
- **Documentation**: 2
- **CI/CD**: 2
- **IDE Config**: 2

## Lines of Code

- **TypeScript/TSX**: ~2,800 lines
- **Configuration**: ~300 lines
- **Documentation**: ~400 lines
- **Total**: ~3,500 lines

## Features Checklist

- [x] Oak/Hono framework integration
- [x] Fresh SSR framework
- [x] Islands architecture
- [x] Deno KV storage
- [x] PostgreSQL/Supabase
- [x] JWT authentication
- [x] WebSocket support
- [x] BroadcastChannel
- [x] Deno.cron jobs
- [x] Middleware stack
- [x] Request validation
- [x] Error handling
- [x] Caching layer
- [x] Database migrations
- [x] Testing setup
- [x] CI/CD pipelines
- [x] Static file serving
- [x] Security headers
- [x] VS Code config
- [x] Comprehensive docs

## Next Steps

1. **Test Templates**: Validate all templates generate correct code
2. **Integration**: Add to `/nuevo-proyecto` wizard
3. **Documentation**: Create video walkthrough
4. **Examples**: Add more real-world examples
5. **Optimization**: Profile and optimize edge performance

## Contributing

To add new templates:

1. Create `.tmpl` file in appropriate directory
2. Use `{{PROJECT_NAME}}` and `{{AUTHOR}}` placeholders
3. Add English code and comments
4. Update this index
5. Add example to EXAMPLES.md
6. Add documentation to README.md

## License

MIT

## Author

Homero Thompson del Lago del Terror

## Resources

- [Deno Documentation](https://deno.land/manual)
- [Deno Deploy](https://deno.com/deploy)
- [Fresh Framework](https://fresh.deno.dev/)
- [Oak Framework](https://oakserver.github.io/oak/)
- [Hono Framework](https://hono.dev/)
