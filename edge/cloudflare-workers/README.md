# {{PROJECT_NAME}} - Cloudflare Workers Edge Functions

Complete Cloudflare Workers edge computing setup with comprehensive features and production-ready patterns.

## Author

{{AUTHOR}}

---

## Features

### Core Infrastructure
- ✅ **Hono Router** - Fast and lightweight router
- ✅ **TypeScript** - Full type safety with strict mode
- ✅ **Vitest** - Modern testing framework
- ✅ **Wrangler 3.x** - Latest Cloudflare Workers tooling

### Storage & Data
- ✅ **KV Namespaces** - Fast key-value storage at the edge
- ✅ **D1 Database** - SQLite database at the edge
- ✅ **R2 Object Storage** - S3-compatible object storage
- ✅ **Durable Objects** - Stateful edge computing

### Features
- ✅ **JWT Authentication** - Secure user authentication
- ✅ **Rate Limiting** - KV-based rate limiting
- ✅ **CORS Handling** - Complete CORS support
- ✅ **WebSocket Support** - Real-time communication
- ✅ **Queues** - Message queue integration
- ✅ **Cron Triggers** - Scheduled tasks
- ✅ **File Uploads** - R2-based file storage
- ✅ **Session Management** - KV-based sessions
- ✅ **Email Queue** - Queue-based email sending

### Security
- ✅ Password hashing with salt
- ✅ JWT token validation
- ✅ API key authentication
- ✅ Webhook signature verification
- ✅ Rate limiting per IP/user
- ✅ Input validation with Zod

---

## Project Structure

```
edge/cloudflare-workers/
├── src/
│   ├── index.ts                  # Main worker entry point
│   ├── router.ts                 # Route configuration
│   ├── handlers/                 # Request handlers
│   │   ├── api.ts               # API endpoints
│   │   ├── auth.ts              # Authentication
│   │   ├── static.ts            # Static assets
│   │   └── websocket.ts         # WebSocket + Durable Objects
│   ├── middleware/              # Middleware functions
│   │   ├── cors.ts              # CORS handling
│   │   ├── auth.ts              # JWT authentication
│   │   ├── rate-limit.ts        # Rate limiting
│   │   └── logger.ts            # Request logging
│   ├── services/                # Business logic
│   │   ├── kv-store.ts          # KV storage utilities
│   │   ├── d1-database.ts       # D1 database utilities
│   │   ├── r2-storage.ts        # R2 storage utilities
│   │   └── queue.ts             # Queue utilities
│   ├── utils/                   # Utility functions
│   │   ├── response.ts          # Response builders
│   │   ├── crypto.ts            # Cryptographic utilities
│   │   └── validation.ts        # Input validation
│   └── types/                   # TypeScript types
│       └── index.ts
├── migrations/                  # D1 database migrations
│   └── 0001_initial.sql
├── test/                        # Test files
│   ├── index.test.ts
│   └── handlers.test.ts
├── wrangler.toml                # Cloudflare Workers config
├── package.json
├── tsconfig.json
└── vitest.config.ts
```

---

## Quick Start

### Prerequisites

- Node.js 18+
- npm or yarn
- Cloudflare account
- Wrangler CLI (`npm install -g wrangler`)

### Installation

```bash
# Install dependencies
npm install

# Login to Cloudflare
wrangler login

# Create KV namespaces
wrangler kv:namespace create CACHE
wrangler kv:namespace create SESSIONS
wrangler kv:namespace create RATE_LIMIT

# Create D1 database
wrangler d1 create {{PROJECT_NAME}}_production

# Create R2 buckets
wrangler r2 bucket create {{PROJECT_NAME}}-assets
wrangler r2 bucket create {{PROJECT_NAME}}-uploads

# Update wrangler.toml with IDs from above commands
```

### Configuration

1. Copy `.env.example` to `.dev.vars`:
```bash
cp .env.example.tmpl .dev.vars
```

2. Update `.dev.vars` with your values:
```env
JWT_SECRET=your-super-secret-key
API_KEY=your-api-key
ALLOWED_ORIGINS=http://localhost:3000
```

3. Update `wrangler.toml` with your Cloudflare IDs:
- Account ID
- KV namespace IDs
- D1 database IDs
- R2 bucket names

### Development

```bash
# Start development server
npm run dev

# The worker will be available at http://localhost:8787
```

### Database Setup

```bash
# Apply migrations
npm run d1:migrations:apply

# Execute SQL file
npm run d1:execute -- ./migrations/0001_initial.sql
```

---

## API Endpoints

### Authentication

```bash
# Register
POST /auth/register
{
  "email": "user@example.com",
  "name": "John Doe",
  "password": "password123"
}

# Login
POST /auth/login
{
  "email": "user@example.com",
  "password": "password123"
}

# Get current user
GET /auth/me
Authorization: Bearer <token>

# Refresh token
POST /auth/refresh
Authorization: Bearer <token>

# Request password reset
POST /auth/password/request-reset
{
  "email": "user@example.com"
}

# Reset password
POST /auth/password/reset
{
  "token": "reset-token",
  "password": "newpassword123"
}

# Change password (authenticated)
POST /auth/password/change
Authorization: Bearer <token>
{
  "currentPassword": "oldpassword",
  "newPassword": "newpassword123"
}
```

### Users API

All user endpoints require authentication.

```bash
# List users
GET /api/v1/users?page=1&limit=20
Authorization: Bearer <token>

# Search users
GET /api/v1/users/search?q=john
Authorization: Bearer <token>

# Get user
GET /api/v1/users/:id
Authorization: Bearer <token>

# Create user
POST /api/v1/users
Authorization: Bearer <token>
{
  "email": "new@example.com",
  "name": "New User"
}

# Update user
PUT /api/v1/users/:id
Authorization: Bearer <token>
{
  "name": "Updated Name"
}

# Delete user (admin only)
DELETE /api/v1/users/:id
Authorization: Bearer <token>
```

### File Upload

```bash
# Upload file
POST /api/v1/upload
Authorization: Bearer <token>
Content-Type: multipart/form-data

file: <binary>
```

### Static Assets

```bash
# Serve static file
GET /static/:path

# Serve image
GET /images/:path

# List assets
GET /assets/list?prefix=uploads&limit=100

# Generate download link
GET /assets/download-link?path=uploads/file.pdf

# Download with token
GET /download/:token
```

### WebSocket

```bash
# Connect to WebSocket
WS /ws

# Connect to chat room
WS /ws/room?room=my-room
```

---

## Bindings

### KV Namespaces

- **CACHE** - General caching
- **SESSIONS** - User sessions
- **RATE_LIMIT** - Rate limiting data

### D1 Database

- **DB** - Main SQLite database

### R2 Buckets

- **ASSETS** - Static assets
- **UPLOADS** - User uploads

### Durable Objects

- **COUNTER** - Counter example
- **CHAT_ROOM** - WebSocket chat rooms

### Queues

- **EMAIL_QUEUE** - Email sending queue

---

## Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run in watch mode
npm run test -- --watch
```

---

## Deployment

### Staging

```bash
npm run deploy:staging
```

### Production

```bash
npm run deploy:production
```

### Secrets Management

```bash
# Set secret
npm run secret:put JWT_SECRET
# Enter secret value when prompted

# List secrets
npm run secret:list
```

---

## Cron Triggers

Configured in `wrangler.toml`:

- `0 */6 * * *` - Every 6 hours: Cleanup task
- `0 0 * * *` - Daily at midnight: Analytics task

---

## Monitoring

### Logs

```bash
# Tail logs
npm run tail

# Tail production logs
npm run tail:production
```

### Analytics

The worker includes Analytics Engine integration for tracking:
- Request metrics
- Error rates
- Performance data

---

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `JWT_SECRET` | Secret for JWT signing | Yes |
| `API_KEY` | API key for authenticated requests | No |
| `ALLOWED_ORIGINS` | CORS allowed origins (comma-separated) | No |
| `RATE_LIMIT_REQUESTS` | Max requests per window | No |
| `RATE_LIMIT_WINDOW` | Rate limit window in seconds | No |
| `LOG_LEVEL` | Logging level (debug, info, warn, error) | No |

---

## Development Guide

### Adding a New Endpoint

1. Create handler in `src/handlers/`
2. Add route in `src/router.ts`
3. Add tests in `test/`

### Adding a New Service

1. Create service in `src/services/`
2. Export from service file
3. Use in handlers

### Adding Middleware

1. Create middleware in `src/middleware/`
2. Apply in router before routes

---

## Performance

- **Cold start**: ~50ms
- **Request latency**: <10ms (edge location)
- **Database queries**: <5ms (D1 SQLite)
- **KV operations**: <1ms

---

## Security Best Practices

1. **Never commit secrets** - Use `wrangler secret put`
2. **Validate all inputs** - Use Zod schemas
3. **Rate limit endpoints** - Prevent abuse
4. **Use HTTPS only** - In production
5. **Sanitize outputs** - Prevent XSS
6. **Rotate secrets** - Regularly update keys

---

## Troubleshooting

### Worker not deploying

```bash
# Check wrangler version
wrangler --version

# Update wrangler
npm install -g wrangler@latest

# Check account authentication
wrangler whoami
```

### KV/D1/R2 not working

- Verify IDs in `wrangler.toml`
- Check permissions in Cloudflare dashboard
- Ensure bindings are configured correctly

### Tests failing

```bash
# Clear cache
rm -rf node_modules/.cache

# Reinstall dependencies
npm install
```

---

## Resources

- [Cloudflare Workers Docs](https://developers.cloudflare.com/workers/)
- [Hono Documentation](https://hono.dev/)
- [D1 Database](https://developers.cloudflare.com/d1/)
- [R2 Storage](https://developers.cloudflare.com/r2/)
- [Durable Objects](https://developers.cloudflare.com/workers/runtime-apis/durable-objects/)

---

## Changelog

### v1.0.0 - 2024-01-15

**Added**
- Initial Cloudflare Workers setup
- JWT authentication
- Rate limiting with KV
- D1 database integration
- R2 file storage
- WebSocket support with Durable Objects
- Queue integration
- Cron triggers
- Comprehensive test suite

---

## License

MIT

---

## Support

For issues and questions:
- Open an issue on GitHub
- Contact: admin@{{PROJECT_NAME}}.com
