# gRPC Backend Templates

> Production-ready gRPC service templates with Protocol Buffers
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for building gRPC services with:

- **Protocol Buffers**: IDL for service definitions
- **Buf**: Modern Protobuf tooling
- **Server Implementations**: Go and Python
- **Client Libraries**: Multiple languages
- **gRPC-Web**: Browser support
- **Interceptors**: Logging, auth, metrics
- **Reflection**: Dynamic service discovery
- **Health Checks**: Standard health protocol

---

## Directory Structure

```
backend/grpc/
├── proto/                          # Protocol Buffer definitions
│   ├── user/
│   │   └── v1/
│   │       └── user.proto.tmpl
│   ├── auth/
│   │   └── v1/
│   │       └── auth.proto.tmpl
│   └── common/
│       └── v1/
│           ├── pagination.proto.tmpl
│           └── timestamp.proto.tmpl
│
├── server/                         # Server implementations
│   ├── go/
│   │   ├── cmd/
│   │   │   └── server/
│   │   │       └── main.go.tmpl
│   │   ├── internal/
│   │   │   ├── services/
│   │   │   │   ├── user.go.tmpl
│   │   │   │   └── auth.go.tmpl
│   │   │   ├── interceptors/
│   │   │   │   ├── auth.go.tmpl
│   │   │   │   └── logging.go.tmpl
│   │   │   └── config/
│   │   │       └── config.go.tmpl
│   │   └── go.mod.tmpl
│   │
│   └── python/
│       ├── src/
│       │   ├── server.py.tmpl
│       │   ├── services/
│       │   │   ├── user.py.tmpl
│       │   │   └── auth.py.tmpl
│       │   └── interceptors/
│       │       ├── auth.py.tmpl
│       │       └── logging.py.tmpl
│       └── pyproject.toml.tmpl
│
├── client/                         # Client examples
│   ├── typescript/
│   │   ├── client.ts.tmpl
│   │   └── package.json.tmpl
│   ├── python/
│   │   └── client.py.tmpl
│   └── go/
│       └── client.go.tmpl
│
├── docker/
│   ├── Dockerfile.go.tmpl
│   ├── Dockerfile.python.tmpl
│   └── docker-compose.grpc.yml.tmpl
│
├── buf.yaml.tmpl                   # Buf configuration
└── buf.gen.yaml.tmpl               # Code generation config
```

---

## Quick Start

### Prerequisites

```bash
# Install Buf
brew install bufbuild/buf/buf

# Or using npm
npm install -g @bufbuild/buf

# Verify installation
buf --version
```

### 1. Define Service (Protocol Buffers)

```protobuf
// proto/user/v1/user.proto
syntax = "proto3";

package user.v1;

option go_package = "github.com/yourorg/api/gen/user/v1;userv1";

service UserService {
  rpc GetUser(GetUserRequest) returns (GetUserResponse);
  rpc ListUsers(ListUsersRequest) returns (ListUsersResponse);
  rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);
  rpc UpdateUser(UpdateUserRequest) returns (UpdateUserResponse);
  rpc DeleteUser(DeleteUserRequest) returns (DeleteUserResponse);
}

message User {
  string id = 1;
  string email = 2;
  string name = 3;
  int64 created_at = 4;
}

message GetUserRequest {
  string id = 1;
}

message GetUserResponse {
  User user = 1;
}

message CreateUserRequest {
  string email = 1;
  string name = 2;
  string password = 3;
}

message CreateUserResponse {
  User user = 1;
}
```

### 2. Configure Buf

```yaml
# buf.yaml
version: v1
breaking:
  use:
    - FILE
lint:
  use:
    - DEFAULT
```

```yaml
# buf.gen.yaml
version: v1
plugins:
  # Go
  - plugin: buf.build/protocolbuffers/go
    out: gen/go
    opt: paths=source_relative
  - plugin: buf.build/grpc/go
    out: gen/go
    opt: paths=source_relative

  # Python
  - plugin: buf.build/protocolbuffers/python
    out: gen/python
  - plugin: buf.build/grpc/python
    out: gen/python

  # TypeScript
  - plugin: buf.build/connectrpc/es
    out: gen/typescript
    opt: target=ts
```

### 3. Generate Code

```bash
# Generate all targets
buf generate

# Generated files:
# gen/go/user/v1/user.pb.go
# gen/go/user/v1/user_grpc.pb.go
# gen/python/user/v1/user_pb2.py
# gen/python/user/v1/user_pb2_grpc.py
```

### 4. Implement Server (Go)

```go
// server/go/internal/services/user.go
package services

import (
    "context"
    userv1 "github.com/yourorg/api/gen/go/user/v1"
)

type UserService struct {
    userv1.UnimplementedUserServiceServer
    db *gorm.DB
}

func (s *UserService) GetUser(
    ctx context.Context,
    req *userv1.GetUserRequest,
) (*userv1.GetUserResponse, error) {
    // Implementation
    user, err := s.db.FindUserByID(ctx, req.Id)
    if err != nil {
        return nil, status.Error(codes.NotFound, "user not found")
    }

    return &userv1.GetUserResponse{
        User: &userv1.User{
            Id:        user.ID,
            Email:     user.Email,
            Name:      user.Name,
            CreatedAt: user.CreatedAt.Unix(),
        },
    }, nil
}
```

### 5. Run Server (Go)

```go
// server/go/cmd/server/main.go
package main

import (
    "log"
    "net"
    "google.golang.org/grpc"
    "google.golang.org/grpc/reflection"
    userv1 "github.com/yourorg/api/gen/go/user/v1"
)

func main() {
    lis, err := net.Listen("tcp", ":50051")
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }

    grpcServer := grpc.NewServer()

    // Register services
    userService := &services.UserService{}
    userv1.RegisterUserServiceServer(grpcServer, userService)

    // Enable reflection
    reflection.Register(grpcServer)

    log.Println("gRPC server listening on :50051")
    if err := grpcServer.Serve(lis); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}
```

### 6. Call from Client (Go)

```go
package main

import (
    "context"
    "log"
    "google.golang.org/grpc"
    userv1 "github.com/yourorg/api/gen/go/user/v1"
)

func main() {
    conn, err := grpc.Dial("localhost:50051", grpc.WithInsecure())
    if err != nil {
        log.Fatal(err)
    }
    defer conn.Close()

    client := userv1.NewUserServiceClient(conn)

    res, err := client.GetUser(context.Background(), &userv1.GetUserRequest{
        Id: "123",
    })
    if err != nil {
        log.Fatal(err)
    }

    log.Printf("User: %+v", res.User)
}
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-grpc-service` |
| `{{AUTHOR}}` | Author name | `Your Name` |
| `{{GO_MODULE}}` | Go module path | `github.com/org/api` |
| `{{PACKAGE}}` | Proto package | `myapp.v1` |
| `{{PORT}}` | Server port | `50051` |

---

## Protocol Buffers Best Practices

### Versioning

```protobuf
// Use versioned packages
package user.v1;

// Update to v2 for breaking changes
package user.v2;
```

### Field Numbering

```protobuf
message User {
  // Reserve deprecated fields
  reserved 4, 5;
  reserved "old_field";

  string id = 1;
  string email = 2;
  string name = 3;
  // Never reuse 4, 5
  int64 created_at = 6;
}
```

### Enums

```protobuf
enum UserStatus {
  USER_STATUS_UNSPECIFIED = 0;  // Always have UNSPECIFIED = 0
  USER_STATUS_ACTIVE = 1;
  USER_STATUS_INACTIVE = 2;
  USER_STATUS_SUSPENDED = 3;
}
```

### Timestamps

```protobuf
import "google/protobuf/timestamp.proto";

message User {
  string id = 1;
  google.protobuf.Timestamp created_at = 2;
}
```

### Pagination

```protobuf
message ListUsersRequest {
  int32 page_size = 1;
  string page_token = 2;
}

message ListUsersResponse {
  repeated User users = 1;
  string next_page_token = 2;
}
```

---

## Server Implementation (Python)

### FastAPI + gRPC

```python
# server.py
import grpc
from concurrent import futures
from generated import user_pb2, user_pb2_grpc

class UserService(user_pb2_grpc.UserServiceServicer):
    def GetUser(self, request, context):
        # Implementation
        user = find_user_by_id(request.id)
        if not user:
            context.abort(grpc.StatusCode.NOT_FOUND, "User not found")

        return user_pb2.GetUserResponse(
            user=user_pb2.User(
                id=user.id,
                email=user.email,
                name=user.name,
                created_at=int(user.created_at.timestamp())
            )
        )

def serve():
    server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
    user_pb2_grpc.add_UserServiceServicer_to_server(UserService(), server)
    server.add_insecure_port('[::]:50051')
    server.start()
    server.wait_for_termination()

if __name__ == '__main__':
    serve()
```

---

## Client Libraries

### TypeScript (gRPC-Web)

```typescript
import { GrpcWebFetchTransport } from "@protobuf-ts/grpcweb-transport";
import { UserServiceClient } from "./gen/user/v1/user.client";

const transport = new GrpcWebFetchTransport({
  baseUrl: "http://localhost:8080",
});

const client = new UserServiceClient(transport);

const { user } = await client.getUser({ id: "123" });
console.log(user);
```

### Python Client

```python
import grpc
from generated import user_pb2, user_pb2_grpc

channel = grpc.insecure_channel('localhost:50051')
stub = user_pb2_grpc.UserServiceStub(channel)

response = stub.GetUser(user_pb2.GetUserRequest(id='123'))
print(response.user)
```

---

## Interceptors

### Authentication (Go)

```go
func AuthInterceptor(
    ctx context.Context,
    req interface{},
    info *grpc.UnaryServerInfo,
    handler grpc.UnaryHandler,
) (interface{}, error) {
    // Extract metadata
    md, ok := metadata.FromIncomingContext(ctx)
    if !ok {
        return nil, status.Error(codes.Unauthenticated, "missing metadata")
    }

    // Validate token
    tokens := md["authorization"]
    if len(tokens) == 0 {
        return nil, status.Error(codes.Unauthenticated, "missing token")
    }

    user, err := validateToken(tokens[0])
    if err != nil {
        return nil, status.Error(codes.Unauthenticated, "invalid token")
    }

    // Add user to context
    ctx = context.WithValue(ctx, "user", user)
    return handler(ctx, req)
}
```

### Logging (Go)

```go
func LoggingInterceptor(
    ctx context.Context,
    req interface{},
    info *grpc.UnaryServerInfo,
    handler grpc.UnaryHandler,
) (interface{}, error) {
    start := time.Now()

    resp, err := handler(ctx, req)

    logger.Info("gRPC call",
        zap.String("method", info.FullMethod),
        zap.Duration("duration", time.Since(start)),
        zap.Error(err),
    )

    return resp, err
}
```

---

## Advanced Features

### Streaming

```protobuf
service ChatService {
  // Server streaming
  rpc GetMessages(GetMessagesRequest) returns (stream Message);

  // Client streaming
  rpc SendMessages(stream Message) returns (SendMessagesResponse);

  // Bidirectional streaming
  rpc Chat(stream Message) returns (stream Message);
}
```

### Error Handling

```go
import "google.golang.org/grpc/status"
import "google.golang.org/grpc/codes"

// Return error
return nil, status.Error(codes.NotFound, "user not found")

// With details
st := status.New(codes.InvalidArgument, "invalid email")
st, _ = st.WithDetails(&errdetails.BadRequest{
    FieldViolations: []*errdetails.BadRequest_FieldViolation{{
        Field:       "email",
        Description: "must be valid email",
    }},
})
return nil, st.Err()
```

### Deadlines

```go
// Server: respect deadline
ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
defer cancel()

// Client: set deadline
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

res, err := client.GetUser(ctx, req)
```

---

## Testing

### Go Server Tests

```go
func TestUserService_GetUser(t *testing.T) {
    s := grpc.NewServer()
    userv1.RegisterUserServiceServer(s, &UserService{})

    lis := bufconn.Listen(1024 * 1024)
    go s.Serve(lis)
    defer s.Stop()

    conn, _ := grpc.Dial("",
        grpc.WithContextDialer(func(context.Context, string) (net.Conn, error) {
            return lis.Dial()
        }),
        grpc.WithInsecure(),
    )
    defer conn.Close()

    client := userv1.NewUserServiceClient(conn)

    res, err := client.GetUser(context.Background(), &userv1.GetUserRequest{
        Id: "123",
    })

    assert.NoError(t, err)
    assert.NotNil(t, res.User)
}
```

---

## Deployment

### Docker

```dockerfile
# Multi-stage build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o server cmd/server/main.go

FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=builder /app/server .
EXPOSE 50051
CMD ["./server"]
```

### Kubernetes

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grpc-service
spec:
  type: ClusterIP
  ports:
    - port: 50051
      targetPort: 50051
      protocol: TCP
      name: grpc
  selector:
    app: grpc-service
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grpc-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: grpc-service
  template:
    metadata:
      labels:
        app: grpc-service
    spec:
      containers:
      - name: server
        image: myregistry/grpc-service:latest
        ports:
        - containerPort: 50051
        env:
        - name: PORT
          value: "50051"
```

---

## Tools

### grpcurl

```bash
# Install
brew install grpcurl

# List services
grpcurl -plaintext localhost:50051 list

# Describe service
grpcurl -plaintext localhost:50051 describe user.v1.UserService

# Call method
grpcurl -plaintext -d '{"id":"123"}' \
  localhost:50051 user.v1.UserService/GetUser
```

### Evans (CLI Client)

```bash
# Install
brew tap ktr0731/evans
brew install evans

# Connect
evans -p 50051 repl

# Call
call GetUser
id (TYPE_STRING) => 123
```

---

## Resources

### Documentation

- [gRPC](https://grpc.io/docs/)
- [Protocol Buffers](https://protobuf.dev/)
- [Buf](https://buf.build/docs/)
- [gRPC-Go](https://github.com/grpc/grpc-go)
- [gRPC-Python](https://grpc.github.io/grpc/python/)

### Tools

- [Buf Schema Registry](https://buf.build/)
- [grpcurl](https://github.com/fullstorydev/grpcurl)
- [Evans](https://github.com/ktr0731/evans)
- [BloomRPC](https://github.com/bloomrpc/bloomrpc)

---

## Support

For issues or questions:
1. Check the gRPC documentation
2. Review Buf guides
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
