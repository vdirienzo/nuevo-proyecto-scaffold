# Go Backend Templates

> Production-ready Go backend templates with Gin framework
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for building Go REST APIs with:

- **Framework**: Gin Web Framework
- **ORM**: GORM (PostgreSQL, MySQL support)
- **Authentication**: JWT with RS256
- **Configuration**: Viper for env management
- **Logging**: Zap structured logging
- **Validation**: go-playground/validator
- **Testing**: testify, httptest
- **Documentation**: Swagger/OpenAPI
- **Containerization**: Multi-stage Docker builds

---

## Directory Structure

```
backend/go/
├── api/                        # API routes and handlers
│   ├── handlers/
│   │   ├── auth.go.tmpl
│   │   ├── users.go.tmpl
│   │   └── health.go.tmpl
│   ├── middleware/
│   │   ├── auth.go.tmpl
│   │   ├── cors.go.tmpl
│   │   └── logger.go.tmpl
│   └── routes/
│       └── routes.go.tmpl
│
├── config/                     # Configuration
│   └── config.go.tmpl
│
├── models/                     # GORM models
│   ├── user.go.tmpl
│   └── base.go.tmpl
│
├── services/                   # Business logic
│   └── auth.go.tmpl
│
├── utils/                      # Utilities
│   ├── jwt.go.tmpl
│   ├── validator.go.tmpl
│   └── password.go.tmpl
│
├── main.go.tmpl
├── go.mod.tmpl
├── Dockerfile.tmpl
└── Makefile.tmpl
```

---

## Quick Start

### 1. Copy Templates

```bash
# Create your API directory
mkdir -p apps/api
cd apps/api

# Copy Go templates
cp -r backend/go/* .

# Remove .tmpl extensions
for file in $(find . -name "*.tmpl"); do
  mv "$file" "${file%.tmpl}"
done
```

### 2. Replace Variables

```bash
# Replace project-specific variables
find . -type f -exec sed -i \
  -e 's/{{PROJECT_NAME}}/my-api/g' \
  -e 's/{{AUTHOR}}/Your Name/g' \
  -e 's/{{GO_MODULE}}/github.com\/yourorg\/my-api/g' \
  {} +
```

### 3. Install Dependencies

```bash
# Initialize Go module
go mod tidy

# Install development tools
go install github.com/swaggo/swag/cmd/swag@latest
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

### 4. Configure Environment

```bash
# Create .env file
cat > .env <<EOF
PORT=8080
GIN_MODE=debug

DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=myapp
DB_SSL_MODE=disable

JWT_PRIVATE_KEY=path/to/private.pem
JWT_PUBLIC_KEY=path/to/public.pem
JWT_EXPIRY=24h

LOG_LEVEL=debug
EOF
```

### 5. Generate JWT Keys

```bash
# Generate RSA key pair
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem
```

### 6. Run

```bash
# Development mode
go run main.go

# Or use Makefile
make run

# Build and run
make build
./bin/api
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-api` |
| `{{AUTHOR}}` | Project author | `Your Name` |
| `{{GO_MODULE}}` | Go module path | `github.com/org/api` |
| `{{DB_HOST}}` | Database host | `localhost` |
| `{{DB_PORT}}` | Database port | `5432` |
| `{{DB_NAME}}` | Database name | `myapp` |
| `{{DB_USER}}` | Database user | `postgres` |
| `{{DB_PASSWORD}}` | Database password | `secret` |

---

## Tech Stack

### Core Dependencies

```go
require (
    github.com/gin-gonic/gin v1.9.1          // Web framework
    gorm.io/gorm v1.25.5                     // ORM
    gorm.io/driver/postgres v1.5.4           // PostgreSQL driver
    github.com/golang-jwt/jwt/v5 v5.2.0      // JWT
    github.com/spf13/viper v1.18.2           // Configuration
    go.uber.org/zap v1.26.0                  // Logging
    github.com/go-playground/validator/v10   // Validation
    golang.org/x/crypto v0.17.0              // Password hashing
)
```

### Development Dependencies

```go
require (
    github.com/stretchr/testify v1.8.4       // Testing
    github.com/swaggo/swag v1.16.2           // Swagger docs
    github.com/swaggo/gin-swagger v1.6.0     // Swagger UI
    github.com/golangci/golangci-lint        // Linting
)
```

---

## Project Structure Explained

### Handlers

HTTP request handlers with input validation:

```go
// Example: CreateUser handler
func (h *UserHandler) CreateUser(c *gin.Context) {
    var input CreateUserInput
    if err := c.ShouldBindJSON(&input); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }

    user, err := h.service.CreateUser(c, input)
    if err != nil {
        c.JSON(500, gin.H{"error": err.Error()})
        return
    }

    c.JSON(201, user)
}
```

### Middleware

- **Auth**: JWT validation
- **CORS**: Cross-origin resource sharing
- **Logger**: Request/response logging
- **Recovery**: Panic recovery
- **Rate Limit**: Request throttling

### Models

GORM models with hooks:

```go
type User struct {
    ID        uint           `gorm:"primarykey" json:"id"`
    Email     string         `gorm:"uniqueIndex;not null" json:"email"`
    Password  string         `gorm:"not null" json:"-"`
    CreatedAt time.Time      `json:"created_at"`
    UpdatedAt time.Time      `json:"updated_at"`
    DeletedAt gorm.DeletedAt `gorm:"index" json:"-"`
}

func (u *User) BeforeCreate(tx *gorm.DB) error {
    // Hash password before saving
    hashed, err := bcrypt.GenerateFromPassword([]byte(u.Password), 14)
    if err != nil {
        return err
    }
    u.Password = string(hashed)
    return nil
}
```

### Services

Business logic layer:

```go
type AuthService struct {
    db     *gorm.DB
    jwt    *JWTService
    logger *zap.Logger
}

func (s *AuthService) Login(ctx context.Context, email, password string) (*Token, error) {
    // 1. Find user
    // 2. Verify password
    // 3. Generate JWT
    // 4. Return token
}
```

---

## Features

### Authentication & Authorization

- JWT with RS256 signing
- Refresh tokens
- Password hashing with bcrypt
- Protected routes middleware

### Database

- GORM ORM with migrations
- Connection pooling
- Soft deletes
- Transactions support

### Validation

- Request validation with struct tags
- Custom validators
- Error messages

### Logging

- Structured logging with Zap
- Request ID tracking
- JSON output for production

### API Documentation

- Swagger/OpenAPI generation
- Interactive API explorer
- Request/response schemas

### Testing

- Unit tests
- Integration tests
- HTTP mocking
- Database mocking

---

## API Endpoints

### Authentication

```
POST   /api/v1/auth/register    # Register new user
POST   /api/v1/auth/login        # Login
POST   /api/v1/auth/refresh      # Refresh token
POST   /api/v1/auth/logout       # Logout
```

### Users

```
GET    /api/v1/users             # List users (admin)
GET    /api/v1/users/:id         # Get user
PUT    /api/v1/users/:id         # Update user
DELETE /api/v1/users/:id         # Delete user
GET    /api/v1/users/me          # Current user
```

### Health

```
GET    /health                   # Health check
GET    /metrics                  # Prometheus metrics
```

---

## Configuration

### Environment Variables

```bash
# Server
PORT=8080
GIN_MODE=release                # debug | release | test

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=secret
DB_NAME=myapp
DB_SSL_MODE=disable
DB_MAX_CONNECTIONS=100
DB_MAX_IDLE=10
DB_MAX_LIFETIME=1h

# JWT
JWT_PRIVATE_KEY=./keys/private.pem
JWT_PUBLIC_KEY=./keys/public.pem
JWT_EXPIRY=24h
JWT_REFRESH_EXPIRY=168h        # 7 days

# Logging
LOG_LEVEL=info                  # debug | info | warn | error

# CORS
CORS_ORIGINS=http://localhost:3000,https://example.com
```

### Viper Configuration

```go
viper.SetConfigName("config")
viper.SetConfigType("yaml")
viper.AddConfigPath(".")
viper.AutomaticEnv()
```

---

## Docker

### Development

```dockerfile
FROM golang:1.21-alpine

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN go build -o bin/api main.go

EXPOSE 8080

CMD ["./bin/api"]
```

### Production (Multi-stage)

```dockerfile
# Builder
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -ldflags="-s -w" -o bin/api main.go

# Runtime
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/bin/api .
EXPOSE 8080
CMD ["./api"]
```

### Docker Compose

```yaml
services:
  api:
    build: .
    ports:
      - "8080:8080"
    environment:
      DB_HOST: postgres
    depends_on:
      - postgres

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data
```

---

## Makefile Commands

```bash
make run          # Run in development mode
make build        # Build binary
make test         # Run tests
make test-cover   # Run tests with coverage
make lint         # Run linter
make swagger      # Generate Swagger docs
make migrate      # Run database migrations
make docker-build # Build Docker image
make clean        # Clean build artifacts
```

---

## Testing

### Unit Tests

```go
func TestCreateUser(t *testing.T) {
    // Arrange
    mockDB, _ := gorm.Open(sqlite.Open(":memory:"), &gorm.Config{})
    service := NewUserService(mockDB)

    // Act
    user, err := service.CreateUser(context.Background(), CreateUserInput{
        Email:    "test@example.com",
        Password: "password123",
    })

    // Assert
    assert.NoError(t, err)
    assert.NotNil(t, user)
    assert.Equal(t, "test@example.com", user.Email)
}
```

### Integration Tests

```go
func TestLoginEndpoint(t *testing.T) {
    // Setup
    router := setupRouter()
    w := httptest.NewRecorder()

    // Request
    body := `{"email":"test@example.com","password":"password"}`
    req, _ := http.NewRequest("POST", "/api/v1/auth/login", strings.NewReader(body))
    req.Header.Set("Content-Type", "application/json")

    // Execute
    router.ServeHTTP(w, req)

    // Assert
    assert.Equal(t, 200, w.Code)
}
```

---

## Best Practices

### Error Handling

```go
// Use custom error types
type AppError struct {
    Code    int
    Message string
    Err     error
}

// Centralized error handling
func HandleError(c *gin.Context, err error) {
    if appErr, ok := err.(*AppError); ok {
        c.JSON(appErr.Code, gin.H{"error": appErr.Message})
    } else {
        c.JSON(500, gin.H{"error": "Internal server error"})
    }
}
```

### Context Usage

```go
// Always pass context
func (s *Service) GetUser(ctx context.Context, id uint) (*User, error) {
    var user User
    err := s.db.WithContext(ctx).First(&user, id).Error
    return &user, err
}
```

### Graceful Shutdown

```go
srv := &http.Server{
    Addr:    ":8080",
    Handler: router,
}

go func() {
    if err := srv.ListenAndServe(); err != nil {
        log.Fatal(err)
    }
}()

quit := make(chan os.Signal, 1)
signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
<-quit

ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()
srv.Shutdown(ctx)
```

---

## Migration to Other Stacks

### From Node.js/Express

- Gin router similar to Express
- Middleware pattern identical
- GORM comparable to Sequelize/TypeORM

### From Python/FastAPI

- Similar automatic validation
- OpenAPI documentation
- Dependency injection pattern

---

## Resources

### Documentation

- [Gin](https://gin-gonic.com/docs/)
- [GORM](https://gorm.io/docs/)
- [Viper](https://github.com/spf13/viper)
- [Zap](https://github.com/uber-go/zap)
- [JWT-Go](https://github.com/golang-jwt/jwt)

### Tools

- [Go Playground](https://play.golang.org/)
- [Go by Example](https://gobyexample.com/)
- [Awesome Go](https://awesome-go.com/)

---

## Troubleshooting

### Port Already in Use

```bash
# Find and kill process
lsof -i :8080
kill -9 <PID>
```

### Database Connection Failed

```bash
# Check PostgreSQL is running
docker ps | grep postgres

# Test connection
psql -h localhost -U postgres -d myapp
```

### JWT Verification Failed

```bash
# Verify keys exist and are readable
ls -la keys/
openssl rsa -in private.pem -check
```

---

## Support

For issues or questions:
1. Check the Go documentation
2. Review Gin framework guides
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
