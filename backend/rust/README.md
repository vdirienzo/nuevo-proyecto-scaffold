# Rust Backend Templates

Complete, production-ready Rust backend templates for the nuevo-proyecto scaffold. Two framework options available: **Actix-web** and **Rocket**.

## Author

Homero Thompson del Lago del Terror

---

## Overview

Both templates provide:

- ✅ Modern Rust 2021 edition practices
- ✅ Async database connections (sqlx + PostgreSQL)
- ✅ JWT authentication with secure password hashing
- ✅ Comprehensive error handling
- ✅ Structured logging with tracing
- ✅ Request validation
- ✅ OpenAPI documentation (Swagger UI)
- ✅ Health check endpoints
- ✅ Docker support with multi-stage builds
- ✅ CORS middleware
- ✅ Type-safe configuration

---

## Framework Comparison

| Feature | Actix-web | Rocket | Winner |
|---------|-----------|--------|--------|
| **Performance** | Extremely fast, async from ground up | Very fast, async since v0.5 | Actix-web |
| **Maturity** | Production-proven, v4.x stable | Stable v0.5, mature ecosystem | Tie |
| **Learning Curve** | Moderate, more boilerplate | Easier, cleaner syntax | Rocket |
| **Type Safety** | Strong, manual guards | Strong, excellent request guards | Rocket |
| **Middleware** | Rich ecosystem | Built-in fairings | Actix-web |
| **Documentation** | Excellent | Excellent | Tie |
| **Community** | Larger, more active | Growing steadily | Actix-web |
| **Async/Await** | Native, requires tokio | Native, built-in runtime | Tie |
| **Macros** | Attribute-based routes | Attribute-based routes | Tie |
| **Testing** | actix-web-test | rocket-test | Tie |

### When to Choose Actix-web

- You need maximum performance for high-traffic applications
- You're building microservices with heavy I/O
- You prefer more control over middleware and extractors
- You need compatibility with actix-actor ecosystem
- Team has experience with async Rust

### When to Choose Rocket

- You prioritize developer experience and clean code
- You want excellent compile-time guarantees
- You prefer less boilerplate and more ergonomic APIs
- You're building a REST API or web service
- Team is new to Rust web development

---

## Quick Start

### Actix-web

```bash
cd actix-web

# Copy environment variables
cp .env.example .env

# Edit database URL and JWT secret
nano .env

# Run with Docker Compose
docker-compose up -d

# Or run locally
cargo run
```

### Rocket

```bash
cd rocket

# Copy environment variables
cp .env.example .env

# Edit database URL and JWT secret
nano .env

# Run with Docker
docker build -t my-rocket-app .
docker run -p 8000:8000 --env-file .env my-rocket-app

# Or run locally
cargo run
```

---

## Project Structure

Both frameworks share similar structure:

```
src/
├── main.rs              # Application entry point
├── config.rs            # Configuration management
├── error.rs             # Error types and handling
├── db/
│   ├── mod.rs
│   └── postgres.rs      # Database connection pool
├── models/
│   ├── mod.rs
│   ├── user.rs          # User model and DTOs
│   └── response.rs      # Standard API responses
├── services/
│   ├── mod.rs
│   └── auth_service.rs  # Authentication business logic
├── middleware/          # Actix-web only
│   ├── mod.rs
│   └── auth.rs          # JWT authentication middleware
├── guards/              # Rocket only
│   ├── mod.rs
│   └── auth.rs          # JWT authentication guard
└── routes/
    ├── mod.rs
    ├── health.rs        # Health check endpoint
    ├── auth.rs          # Login/register endpoints
    └── users.rs         # User management endpoints
```

---

## API Endpoints

Both templates implement identical APIs:

### Health Check

```
GET /api/v1/health
```

Response:
```json
{
  "status": "ok",
  "database": "healthy"
}
```

### Authentication

#### Register

```
POST /api/v1/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "username": "johndoe",
  "password": "secure_password"
}
```

Response:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "username": "johndoe",
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

#### Login

```
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secure_password"
}
```

Response: Same as register

### Users

#### Get All Users

```
GET /api/v1/users
```

#### Get User by ID

```
GET /api/v1/users/{id}
```

#### Update User

```
PUT /api/v1/users/{id}
Content-Type: application/json

{
  "email": "newemail@example.com",
  "username": "newusername"
}
```

---

## Authentication

Both templates use JWT tokens with Bearer authentication:

```bash
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  http://localhost:8000/api/v1/users
```

### Password Security

- Passwords are hashed using **Argon2** (OWASP recommended)
- Salts are automatically generated
- Password verification is timing-attack resistant

---

## Database Migrations

Create a migration:

```bash
# Install sqlx-cli
cargo install sqlx-cli --no-default-features --features postgres

# Create migration
sqlx migrate add create_users_table

# Edit the migration file in migrations/
# Then apply:
sqlx migrate run
```

Example migration (`migrations/001_create_users_table.sql`):

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL,
    updated_at TIMESTAMPTZ NOT NULL
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
```

---

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `HOST` | Server host (Actix only) | `0.0.0.0` |
| `PORT` | Server port (Actix only) | `8000` |
| `DATABASE_URL` | PostgreSQL connection string | **Required** |
| `JWT_SECRET` | Secret key for JWT signing | **Required** |
| `JWT_EXPIRATION_HOURS` | Token expiration time | `24` |
| `RUST_LOG` | Log level | `info` |

### Rocket Configuration

Rocket uses `Rocket.toml` for additional configuration:

```toml
[default]
address = "0.0.0.0"
port = 8000
log_level = "normal"
```

---

## Docker Deployment

Both templates include optimized multi-stage Dockerfiles:

### Build

```bash
# Actix-web
docker build -t my-actix-app ./actix-web

# Rocket
docker build -t my-rocket-app ./rocket
```

### Run

```bash
docker run -d \
  -p 8000:8000 \
  -e DATABASE_URL="postgresql://user:pass@host:5432/db" \
  -e JWT_SECRET="your_secret_key" \
  my-actix-app
```

### Docker Compose

Use the included `docker-compose.yml` (Actix-web):

```bash
cd actix-web
docker-compose up -d
```

This starts both the application and PostgreSQL database.

---

## OpenAPI Documentation

Both templates include Swagger UI documentation:

```
http://localhost:8000/swagger-ui/
```

The OpenAPI specification is auto-generated from code annotations using **utoipa**.

---

## Testing

### Unit Tests

```bash
cargo test
```

### Integration Tests

Create tests in `tests/` directory:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[tokio::test]
    async fn test_health_check() {
        // Test implementation
    }
}
```

### Manual Testing with curl

```bash
# Health check
curl http://localhost:8000/api/v1/health

# Register
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","username":"testuser","password":"password123"}'

# Login
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"password123"}'
```

---

## Logging

Both templates use **tracing** for structured logging:

```rust
use tracing::{info, warn, error, debug};

info!("Server started");
warn!("High memory usage");
error!("Database connection failed");
debug!("Processing request: {:?}", request);
```

### Log Levels

Set via `RUST_LOG` environment variable:

```bash
# Show all logs
RUST_LOG=debug cargo run

# Show only warnings and errors
RUST_LOG=warn cargo run

# Fine-grained control
RUST_LOG=info,my_project=debug cargo run
```

---

## Error Handling

Unified error handling with custom `AppError` type:

```rust
pub enum AppError {
    Database(sqlx::Error),
    Authentication(String),
    Authorization(String),
    Validation(String),
    NotFound(String),
    Conflict(String),
    Internal(String),
    Jwt(jsonwebtoken::errors::Error),
    Argon2(argon2::password_hash::Error),
}
```

All errors automatically convert to appropriate HTTP responses.

---

## Security Best Practices

Both templates implement:

- ✅ Password hashing with Argon2
- ✅ JWT with configurable expiration
- ✅ SQL injection prevention (parameterized queries)
- ✅ Input validation with validator crate
- ✅ CORS configuration
- ✅ Non-root Docker user
- ✅ Secrets from environment variables
- ✅ HTTPS-ready (configure reverse proxy)

### Additional Recommendations

1. **Use HTTPS in production** (nginx/Caddy reverse proxy)
2. **Rotate JWT secrets regularly**
3. **Set short token expiration for sensitive apps**
4. **Implement rate limiting** (use actix-governor or rocket-governor)
5. **Add CSRF protection for web UIs**
6. **Enable database SSL connections**

---

## Performance Tuning

### Database Connection Pool

Adjust pool size in `src/db/postgres.rs`:

```rust
let pool = PgPoolOptions::new()
    .max_connections(20)  // Increase for high traffic
    .connect(database_url)
    .await?;
```

### Actix-web Workers

Set worker threads:

```rust
HttpServer::new(|| { /* ... */ })
    .workers(4)  // Number of worker threads
    .bind((host, port))?
    .run()
    .await
```

### Rocket Limits

Configure in `Rocket.toml`:

```toml
[default.limits]
json = "5MiB"
forms = "1MiB"
```

---

## Migration from One Framework to Another

The templates are structurally similar, making migration straightforward:

### Actix-web → Rocket

1. Replace `actix_web::*` imports with `rocket::*`
2. Convert middleware to guards
3. Change route macros syntax (`#[get("/path")]`)
4. Update `HttpResponse` to `Json<T>` returns
5. Replace `web::Data` with `&State`

### Rocket → Actix-web

1. Replace `rocket::*` imports with `actix_web::*`
2. Convert guards to middleware
3. Update route signatures
4. Change `Json<T>` to `HttpResponse::Ok().json()`
5. Replace `&State` with `web::Data`

---

## Troubleshooting

### Common Issues

**Database connection fails**
```bash
# Check PostgreSQL is running
docker-compose ps

# Verify DATABASE_URL format
# postgresql://user:password@host:port/database
```

**JWT token invalid**
```bash
# Ensure JWT_SECRET matches between requests
# Check token expiration time
```

**Port already in use**
```bash
# Change PORT in .env
PORT=8080

# Or kill existing process
lsof -ti:8000 | xargs kill
```

**Compilation errors**
```bash
# Update dependencies
cargo update

# Clean and rebuild
cargo clean && cargo build
```

---

## Production Checklist

- [ ] Set strong `JWT_SECRET` (32+ characters)
- [ ] Use production database credentials
- [ ] Enable database SSL connections
- [ ] Set up HTTPS with reverse proxy
- [ ] Configure CORS for specific origins
- [ ] Set appropriate `RUST_LOG` level
- [ ] Enable database connection pooling
- [ ] Add health check monitoring
- [ ] Set up backup strategy
- [ ] Configure rate limiting
- [ ] Add observability (metrics, traces)
- [ ] Review and harden Docker image
- [ ] Set resource limits (memory, CPU)
- [ ] Enable security headers
- [ ] Implement graceful shutdown

---

## Resources

### Actix-web

- [Official Documentation](https://actix.rs/docs/)
- [API Reference](https://docs.rs/actix-web/)
- [Examples](https://github.com/actix/examples)

### Rocket

- [Official Documentation](https://rocket.rs/guide/)
- [API Reference](https://docs.rs/rocket/)
- [Examples](https://github.com/rwf2/Rocket/tree/master/examples)

### Dependencies

- [sqlx](https://github.com/launchbadge/sqlx) - Async SQL toolkit
- [jsonwebtoken](https://github.com/Keats/jsonwebtoken) - JWT implementation
- [argon2](https://github.com/RustCrypto/password-hashes) - Password hashing
- [utoipa](https://github.com/juhaku/utoipa) - OpenAPI documentation
- [validator](https://github.com/Keats/validator) - Struct validation

---

## Contributing

To improve these templates:

1. Test changes with both frameworks
2. Update both templates to maintain feature parity
3. Update this README with new features
4. Follow Rust best practices and idioms

---

## License

MIT License - See LICENSE file in repository root

---

## Changelog

### 1.0.0 - 2024-01-19

- Initial release
- Actix-web v4.5 template
- Rocket v0.5 template
- JWT authentication
- PostgreSQL integration
- OpenAPI documentation
- Docker support
- Comprehensive error handling
- Request validation
- Health check endpoints

---

**Author:** Homero Thompson del Lago del Terror
**Last Updated:** 2024-01-19
