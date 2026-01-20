# {{PROJECT_NAME}} - Deno Deploy Edge Application

Production-ready edge computing application built with Deno, Fresh, and Oak framework.

## Features

### Core
- **Oak Framework** - Fast and flexible web framework
- **Fresh** - Modern SSR framework with islands architecture
- **Deno KV** - Built-in key-value storage
- **PostgreSQL/Supabase** - Full database support
- **JWT Authentication** - Secure user authentication
- **WebSocket Support** - Real-time communication with BroadcastChannel
- **Cron Jobs** - Scheduled tasks with Deno.cron

### Architecture
- Islands architecture for optimal performance
- Server-side rendering with Fresh
- Edge-first design for global distribution
- Atomic operations with Deno KV
- Middleware-based request handling

## Prerequisites

- [Deno](https://deno.land/) 2.x or later
- [Deno Deploy](https://deno.com/deploy) account (for deployment)
- PostgreSQL or Supabase account (optional, for database features)

## Quick Start

### 1. Environment Setup

```bash
# Copy environment template
cp .env.example .env

# Edit with your configuration
nano .env
```

### 2. Install Dependencies

Deno automatically manages dependencies. No explicit install step needed.

### 3. Development

```bash
# Start development server with hot reload
deno task dev

# Or using Fresh
deno task fresh:start
```

The server will start at `http://localhost:8000`

### 4. Testing

```bash
# Run all tests
deno task test

# Run specific test file
deno test tests/app_test.ts
```

### 5. Production Build (Fresh)

```bash
# Build Fresh app
deno task fresh:build

# Preview production build
deno task fresh:preview
```

## Project Structure

```
{{PROJECT_NAME}}/
├── main.ts                     # Entry point
├── deno.json                   # Deno configuration
├── import_map.json             # Import aliases
├── .env.example                # Environment template
├── src/
│   ├── app.ts                  # Application setup
│   ├── router.ts               # URL routing
│   ├── handlers/               # Request handlers
│   │   ├── api.ts              # CRUD operations
│   │   ├── auth.ts             # Authentication
│   │   ├── static.ts           # Static files
│   │   └── websocket.ts        # WebSocket handler
│   ├── middleware/             # Middleware stack
│   │   ├── cors.ts             # CORS configuration
│   │   ├── auth.ts             # JWT verification
│   │   ├── logger.ts           # Request logging
│   │   └── error-handler.ts   # Global error handling
│   ├── services/               # Business logic
│   │   ├── kv.ts               # Deno KV wrapper
│   │   ├── database.ts         # PostgreSQL/Supabase
│   │   └── cache.ts            # Caching service
│   ├── utils/                  # Utilities
│   │   ├── response.ts         # API response helpers
│   │   ├── validation.ts       # Request validation
│   │   └── crypto.ts           # Password hashing
│   └── types/                  # TypeScript types
│       └── index.ts
├── islands/                    # Fresh islands (interactive)
│   ├── Counter.tsx
│   └── AuthForm.tsx
├── routes/                     # Fresh routes (SSR)
│   ├── index.tsx
│   ├── api/[...path].ts
│   └── _middleware.ts
├── static/                     # Static assets
│   └── styles.css
└── tests/                      # Test files
    ├── app_test.ts
    └── handlers_test.ts
```

## API Endpoints

### Authentication

```bash
# Register new user
POST /auth/register
Content-Type: application/json
{
  "email": "user@example.com",
  "password": "securepass123",
  "name": "John Doe"
}

# Login
POST /auth/login
Content-Type: application/json
{
  "email": "user@example.com",
  "password": "securepass123"
}

# Refresh token
POST /auth/refresh
Authorization: Bearer <token>

# Get profile
GET /auth/me
Authorization: Bearer <token>
```

### CRUD Operations

```bash
# List items
GET /api/items

# Get single item
GET /api/items/:id

# Create item
POST /api/items
Content-Type: application/json
{
  "name": "Item name",
  "description": "Optional description",
  "status": "active"
}

# Update item
PUT /api/items/:id
Content-Type: application/json
{
  "name": "Updated name",
  "status": "inactive"
}

# Delete item
DELETE /api/items/:id
```

### WebSocket

```javascript
// Connect to WebSocket
const ws = new WebSocket("ws://localhost:8000/ws");

ws.onopen = () => {
  console.log("Connected");
  ws.send(JSON.stringify({ content: "Hello!" }));
};

ws.onmessage = (event) => {
  const message = JSON.parse(event.data);
  console.log("Received:", message);
};
```

## Deno KV Usage

```typescript
import { kvService } from "./src/services/kv.ts";

// Set value
await kvService.set("user:123", { name: "John" });

// Get value
const user = await kvService.get("user:123");

// Delete value
await kvService.delete("user:123");

// List by prefix
const users = await kvService.listByPrefix("user:");

// With expiration (1 hour)
await kvService.set("session:abc", { userId: "123" }, 3600 * 1000);
```

## Database Usage

### PostgreSQL

```typescript
import { query, queryOne } from "./src/services/database.ts";

// Query multiple rows
const users = await query<User>("SELECT * FROM users WHERE active = $1", [true]);

// Query single row
const user = await queryOne<User>("SELECT * FROM users WHERE id = $1", [userId]);
```

### Supabase

```typescript
import { getSupabase } from "./src/services/database.ts";

const supabase = getSupabase();

// Select
const { data, error } = await supabase
  .from("users")
  .select("*")
  .eq("active", true);

// Insert
const { data, error } = await supabase
  .from("users")
  .insert({ email: "user@example.com", name: "John" });
```

## Cron Jobs

Cron jobs are configured in `src/app.ts`:

```typescript
// Daily cleanup at midnight
Deno.cron("cleanup", "0 0 * * *", async () => {
  console.log("Running daily cleanup");
  // Add cleanup logic
});

// Health check every 5 minutes
Deno.cron("health-check", "*/5 * * * *", async () => {
  console.log("Health check ping");
  // Add monitoring logic
});
```

## Deployment

### Deploy to Deno Deploy

1. **Install Deno Deploy CLI**

```bash
deno install --allow-all --global https://deno.land/x/deploy/deployctl.ts
```

2. **Login to Deno Deploy**

```bash
deployctl login
```

3. **Deploy**

```bash
# Deploy from local
deployctl deploy --project={{PROJECT_NAME}} main.ts

# Or deploy from GitHub (recommended)
# Connect your GitHub repository in Deno Deploy dashboard
# https://dash.deno.com/projects
```

4. **Set Environment Variables**

In the Deno Deploy dashboard:
- Go to your project settings
- Add environment variables from `.env.example`
- Save and redeploy

### Deploy with GitHub Actions

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Deno Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - uses: denoland/setup-deno@v1
        with:
          deno-version: v1.x

      - name: Deploy to Deno Deploy
        uses: denoland/deployctl@v1
        with:
          project: {{PROJECT_NAME}}
          entrypoint: main.ts
          root: .
```

Add secrets in GitHub repository settings:
- `DENO_DEPLOY_TOKEN` - Get from Deno Deploy dashboard

### Custom Domain

1. Go to Deno Deploy project settings
2. Add custom domain
3. Update DNS records as instructed
4. Enable automatic HTTPS

## Configuration

### deno.json

Main configuration file with:
- Import maps
- Compiler options
- Tasks
- Formatting rules
- Linting rules

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `APP_ENV` | Environment (development/production) | `development` |
| `PORT` | Server port | `8000` |
| `LOG_LEVEL` | Logging level (ERROR/WARN/INFO/DEBUG) | `INFO` |
| `JWT_SECRET` | JWT signing secret | Required |
| `JWT_EXPIRES_IN` | Token expiration | `7d` |
| `DATABASE_URL` | PostgreSQL connection string | Optional |
| `SUPABASE_URL` | Supabase project URL | Optional |
| `SUPABASE_ANON_KEY` | Supabase anonymous key | Optional |
| `ALLOWED_ORIGINS` | CORS allowed origins | `http://localhost:3000` |

## Performance Optimization

### Edge Caching

```typescript
import { cacheService } from "./src/services/cache.ts";

// Cache-aside pattern
const data = await cacheService.getOrSet(
  "expensive-operation",
  async () => {
    // Expensive computation
    return await fetchFromDatabase();
  },
  3600 * 1000 // 1 hour TTL
);
```

### Deno KV Best Practices

- Use prefixes for key organization (`user:`, `item:`, `session:`)
- Set expiration on temporary data
- Use atomic operations for concurrent updates
- Batch operations when possible

### Fresh Islands

- Keep islands small and focused
- Use signals for reactive state
- Minimize JavaScript sent to client
- Prefer server-side rendering when possible

## Security

### Implemented

- JWT authentication with secure secret
- Password hashing with PBKDF2
- CORS configuration
- Input validation with Zod
- SQL parameterized queries
- Security headers (X-Frame-Options, X-Content-Type-Options)

### Best Practices

- Never commit `.env` file
- Rotate JWT secrets regularly
- Use HTTPS in production
- Implement rate limiting
- Validate all user input
- Use Supabase RLS for database security

## Monitoring

### Logging

Logs are controlled by `LOG_LEVEL` environment variable:

```bash
LOG_LEVEL=DEBUG deno task dev  # Verbose logging
LOG_LEVEL=ERROR deno task dev  # Only errors
```

### Health Check

```bash
curl http://localhost:8000/health

# Response
{
  "status": "healthy",
  "timestamp": "2025-01-19T12:00:00.000Z",
  "service": "{{PROJECT_NAME}}"
}
```

### Deno Deploy Metrics

Access metrics in Deno Deploy dashboard:
- Request count
- Response time
- Error rate
- Geographic distribution

## Troubleshooting

### Common Issues

**Issue: Cannot connect to database**
```bash
# Check DATABASE_URL format
postgresql://user:password@host:5432/database

# Test connection
deno run --allow-net --allow-env test-db.ts
```

**Issue: JWT errors**
```bash
# Ensure JWT_SECRET is set
echo $JWT_SECRET

# Token format must be: Bearer <token>
Authorization: Bearer eyJhbGc...
```

**Issue: CORS errors**
```bash
# Add frontend URL to ALLOWED_ORIGINS
ALLOWED_ORIGINS=http://localhost:3000,https://myapp.com
```

**Issue: KV operations fail**
```bash
# Ensure --unstable-kv flag
deno run --unstable-kv main.ts

# Check deno.json tasks include flag
```

## Development Tips

### Hot Reload

```bash
# Oak with watch mode
deno task dev

# Fresh with watch
deno task fresh:start
```

### Type Checking

```bash
# Check all files
deno task check

# Check specific file
deno check src/app.ts
```

### Formatting

```bash
# Format code
deno task fmt

# Check formatting
deno fmt --check
```

### Linting

```bash
# Lint code
deno task lint

# Auto-fix issues
deno lint --fix
```

## License

MIT

## Author

{{AUTHOR}}

## Resources

- [Deno Documentation](https://deno.land/manual)
- [Deno Deploy](https://deno.com/deploy)
- [Fresh Framework](https://fresh.deno.dev/)
- [Oak Framework](https://oakserver.github.io/oak/)
- [Deno KV Guide](https://deno.com/kv)
- [Supabase](https://supabase.com/)

## Support

For issues and questions:
- Open an issue on GitHub
- Check Deno Discord community
- Review Deno Deploy documentation
