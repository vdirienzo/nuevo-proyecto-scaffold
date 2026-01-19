# Phase 3: Code Generation Examples

**Purpose**: Show how different stack choices generate different project structures.

---

## Example 1: Modern Startup Stack

### Stack Selection

```yaml
architecture_pattern: modular_monolith
backend: fastapi
frontend_platforms: [web, mobile]
web_framework: nextjs
api_style: [rest]
primary_database: supabase
caching: redis
vector_database: pgvector
deployment_strategy: serverless
```

### Generated Project Structure

```
my-startup/
├── apps/
│   ├── web/                      # Next.js 15
│   │   ├── app/
│   │   │   ├── (auth)/
│   │   │   ├── (dashboard)/
│   │   │   ├── api/
│   │   │   └── layout.tsx
│   │   ├── components/
│   │   ├── lib/
│   │   │   ├── supabase.ts
│   │   │   └── api-client.ts
│   │   └── package.json
│   │
│   ├── mobile/                   # React Native + Expo
│   │   ├── src/
│   │   │   ├── screens/
│   │   │   ├── components/
│   │   │   ├── navigation/
│   │   │   └── lib/
│   │   │       ├── supabase.ts
│   │   │       └── api-client.ts
│   │   ├── app.json
│   │   └── package.json
│   │
│   └── api/                      # FastAPI
│       ├── src/
│       │   └── api/
│       │       ├── main.py
│       │       ├── config.py
│       │       ├── modules/      # Modular monolith
│       │       │   ├── auth/
│       │       │   ├── users/
│       │       │   ├── products/
│       │       │   └── ai/       # AI features
│       │       │       ├── embeddings.py
│       │       │       └── search.py
│       │       ├── shared/
│       │       │   ├── database.py
│       │       │   ├── cache.py  # Redis
│       │       │   └── vector.py # pgvector
│       │       └── tests/
│       ├── pyproject.toml
│       └── Dockerfile
│
├── packages/
│   ├── shared-types/             # Shared TypeScript types
│   └── ui/                       # Shared UI components
│
├── docker-compose.yml            # Local dev (Redis)
├── vercel.json                   # Serverless deployment
└── turbo.json
```

### Key Generated Files

#### `apps/api/src/api/shared/vector.py` (AI-ready)

```python
"""
vector.py - pgvector integration for semantic search

Autor: Homero Thompson del Lago del Terror
"""

from sqlalchemy import select, func
from pgvector.sqlalchemy import Vector
from sqlmodel import Field, SQLModel
from openai import AsyncOpenAI

client = AsyncOpenAI()

class Document(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    content: str
    embedding: list[float] = Field(sa_column=Vector(1536))

async def generate_embedding(text: str) -> list[float]:
    """Generate OpenAI embedding."""
    response = await client.embeddings.create(
        model="text-embedding-3-small",
        input=text
    )
    return response.data[0].embedding

async def semantic_search(query: str, limit: int = 10):
    """Semantic search using pgvector."""
    query_embedding = await generate_embedding(query)

    # Cosine similarity search
    stmt = (
        select(Document)
        .order_by(Document.embedding.cosine_distance(query_embedding))
        .limit(limit)
    )
    return await session.exec(stmt)
```

#### `apps/api/src/api/shared/cache.py` (Redis)

```python
"""
cache.py - Redis caching utilities

Autor: Homero Thompson del Lago del Terror
"""

from redis.asyncio import Redis
from functools import wraps
import json

redis = Redis.from_url("redis://localhost:6379")

def cached(ttl: int = 300):
    """Cache decorator."""
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            cache_key = f"{func.__name__}:{args}:{kwargs}"

            # Try cache
            cached_value = await redis.get(cache_key)
            if cached_value:
                return json.loads(cached_value)

            # Execute and cache
            result = await func(*args, **kwargs)
            await redis.setex(cache_key, ttl, json.dumps(result))
            return result
        return wrapper
    return decorator

# Usage
@cached(ttl=600)
async def get_popular_products():
    # Expensive DB query
    pass
```

#### `vercel.json` (Serverless deployment)

```json
{
  "version": 2,
  "builds": [
    {
      "src": "apps/web/package.json",
      "use": "@vercel/next"
    },
    {
      "src": "apps/api/src/api/main.py",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "apps/api/src/api/main.py"
    }
  ]
}
```

---

## Example 2: High-Performance Microservices

### Stack Selection

```yaml
architecture_pattern: microservices
backend: go_fiber
api_style: [rest, grpc]
primary_database: postgresql
caching: redis
deployment_strategy: traditional
```

### Generated Project Structure

```
my-platform/
├── services/
│   ├── gateway/                  # API Gateway (Go + Fiber)
│   │   ├── cmd/
│   │   │   └── main.go
│   │   ├── internal/
│   │   │   ├── router/
│   │   │   ├── middleware/
│   │   │   └── proxy/
│   │   ├── go.mod
│   │   └── Dockerfile
│   │
│   ├── auth/                     # Auth service (Go + gRPC)
│   │   ├── cmd/
│   │   │   └── main.go
│   │   ├── internal/
│   │   │   ├── service/
│   │   │   ├── repository/
│   │   │   └── cache/        # Redis
│   │   ├── proto/
│   │   │   └── auth.proto
│   │   ├── go.mod
│   │   └── Dockerfile
│   │
│   ├── users/                    # Users service (Go + gRPC)
│   │   └── ...
│   │
│   └── products/                 # Products service (Go + gRPC)
│       └── ...
│
├── proto/                        # Shared protobuf definitions
│   ├── auth.proto
│   ├── users.proto
│   └── products.proto
│
├── k8s/                          # Kubernetes manifests
│   ├── gateway/
│   ├── auth/
│   ├── users/
│   ├── products/
│   └── infrastructure/
│       ├── postgresql.yaml
│       └── redis.yaml
│
└── docker-compose.yml
```

### Key Generated Files

#### `services/gateway/cmd/main.go` (API Gateway)

```go
package main

import (
    "github.com/gofiber/fiber/v2"
    "github.com/gofiber/fiber/v2/middleware/logger"
    "google.golang.org/grpc"
)

func main() {
    app := fiber.New()

    // Middleware
    app.Use(logger.New())

    // gRPC clients
    authConn, _ := grpc.Dial("auth-service:50051", grpc.WithInsecure())
    authClient := authpb.NewAuthServiceClient(authConn)

    // REST → gRPC proxy
    app.Post("/api/auth/login", func(c *fiber.Ctx) error {
        req := new(LoginRequest)
        if err := c.BodyParser(req); err != nil {
            return err
        }

        // Call gRPC service
        resp, err := authClient.Login(c.Context(), &authpb.LoginRequest{
            Email:    req.Email,
            Password: req.Password,
        })

        if err != nil {
            return c.Status(500).JSON(fiber.Map{"error": err.Error()})
        }

        return c.JSON(fiber.Map{
            "token": resp.Token,
            "user": resp.User,
        })
    })

    app.Listen(":3000")
}
```

#### `services/auth/internal/cache/redis.go` (Redis caching)

```go
package cache

import (
    "context"
    "time"
    "github.com/redis/go-redis/v9"
)

type Cache struct {
    client *redis.Client
}

func NewCache() *Cache {
    return &Cache{
        client: redis.NewClient(&redis.Options{
            Addr: "redis:6379",
        }),
    }
}

func (c *Cache) Get(ctx context.Context, key string) (string, error) {
    return c.client.Get(ctx, key).Result()
}

func (c *Cache) Set(ctx context.Context, key string, value string, ttl time.Duration) error {
    return c.client.Set(ctx, key, value, ttl).Err()
}

// Usage in service
func (s *AuthService) ValidateToken(ctx context.Context, token string) (*User, error) {
    // Try cache first
    userID, err := s.cache.Get(ctx, "token:"+token)
    if err == nil {
        return s.repo.GetUser(ctx, userID)
    }

    // Validate token and cache result
    user, err := s.validateAndDecodeToken(token)
    if err != nil {
        return nil, err
    }

    s.cache.Set(ctx, "token:"+token, user.ID, 15*time.Minute)
    return user, nil
}
```

#### `proto/auth.proto` (gRPC contract)

```protobuf
syntax = "proto3";

package auth;

option go_package = "github.com/myplatform/proto/authpb";

service AuthService {
  rpc Login(LoginRequest) returns (LoginResponse);
  rpc Validate(ValidateRequest) returns (ValidateResponse);
  rpc Refresh(RefreshRequest) returns (RefreshResponse);
}

message LoginRequest {
  string email = 1;
  string password = 2;
}

message LoginResponse {
  string token = 1;
  string refresh_token = 2;
  User user = 3;
}

message User {
  string id = 1;
  string email = 2;
  string name = 3;
}
```

#### `k8s/gateway/deployment.yaml` (K8s manifest)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gateway
spec:
  replicas: 3
  selector:
    matchLabels:
      app: gateway
  template:
    metadata:
      labels:
        app: gateway
    spec:
      containers:
      - name: gateway
        image: myplatform/gateway:latest
        ports:
        - containerPort: 3000
        env:
        - name: AUTH_SERVICE_URL
          value: "auth-service:50051"
        - name: USERS_SERVICE_URL
          value: "users-service:50051"
        resources:
          limits:
            memory: "256Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: gateway
spec:
  selector:
    app: gateway
  ports:
  - port: 80
    targetPort: 3000
  type: LoadBalancer
```

---

## Example 3: AI-First Application with GraphQL

### Stack Selection

```yaml
architecture_pattern: modular_monolith
backend: fastapi
frontend_platforms: [web]
web_framework: nextjs
api_style: [rest, graphql]
primary_database: postgresql
caching: redis
vector_database: pgvector
search_engine: elasticsearch
deployment_strategy: serverless
```

### Generated Project Structure

```
ai-platform/
├── apps/
│   ├── web/                      # Next.js + Apollo Client
│   │   ├── app/
│   │   ├── components/
│   │   ├── lib/
│   │   │   ├── apollo-client.ts
│   │   │   └── graphql/
│   │   │       ├── queries.ts
│   │   │       └── mutations.ts
│   │   └── package.json
│   │
│   └── api/                      # FastAPI + Strawberry GraphQL
│       ├── src/
│       │   └── api/
│       │       ├── main.py
│       │       ├── graphql/      # GraphQL layer
│       │       │   ├── schema.py
│       │       │   ├── queries.py
│       │       │   ├── mutations.py
│       │       │   └── types.py
│       │       ├── rest/         # REST endpoints
│       │       │   └── routes/
│       │       ├── ai/           # AI modules
│       │       │   ├── embeddings.py
│       │       │   ├── search.py
│       │       │   └── rag.py
│       │       ├── shared/
│       │       │   ├── database.py
│       │       │   ├── cache.py
│       │       │   ├── vector.py
│       │       │   └── search.py  # Elasticsearch
│       │       └── tests/
│       └── pyproject.toml
│
└── docker-compose.yml
```

### Key Generated Files

#### `apps/api/src/api/graphql/schema.py` (Strawberry GraphQL)

```python
"""
schema.py - GraphQL schema

Autor: Homero Thompson del Lago del Terror
"""

import strawberry
from typing import List

@strawberry.type
class User:
    id: int
    email: str
    name: str

@strawberry.type
class Document:
    id: int
    title: str
    content: str
    similarity_score: float | None = None

@strawberry.type
class Query:
    @strawberry.field
    async def users(self) -> List[User]:
        """Get all users."""
        return await get_users()

    @strawberry.field
    async def semantic_search(self, query: str, limit: int = 10) -> List[Document]:
        """Semantic search using pgvector."""
        return await vector_search(query, limit)

    @strawberry.field
    async def full_text_search(self, query: str) -> List[Document]:
        """Full-text search using Elasticsearch."""
        return await elasticsearch_search(query)

@strawberry.type
class Mutation:
    @strawberry.mutation
    async def create_user(self, email: str, name: str) -> User:
        """Create new user."""
        return await create_user_db(email, name)

    @strawberry.mutation
    async def index_document(self, title: str, content: str) -> Document:
        """Index document in both pgvector and Elasticsearch."""
        doc = await save_document(title, content)

        # Generate embedding
        embedding = await generate_embedding(content)
        await save_embedding(doc.id, embedding)

        # Index in Elasticsearch
        await index_in_elasticsearch(doc)

        return doc

schema = strawberry.Schema(query=Query, mutation=Mutation)
```

#### `apps/api/src/api/ai/rag.py` (RAG implementation)

```python
"""
rag.py - Retrieval Augmented Generation

Autor: Homero Thompson del Lago del Terror
"""

from openai import AsyncOpenAI
from .search import semantic_search

client = AsyncOpenAI()

async def rag_query(question: str, context_limit: int = 5) -> str:
    """
    RAG query: retrieve relevant docs + generate answer.
    """
    # 1. Retrieve relevant documents
    docs = await semantic_search(question, limit=context_limit)

    # 2. Build context
    context = "\n\n".join([f"Document {i+1}:\n{doc.content}"
                           for i, doc in enumerate(docs)])

    # 3. Generate answer
    messages = [
        {
            "role": "system",
            "content": "You are a helpful assistant. Answer based on the provided context."
        },
        {
            "role": "user",
            "content": f"Context:\n{context}\n\nQuestion: {question}"
        }
    ]

    response = await client.chat.completions.create(
        model="gpt-4-turbo-preview",
        messages=messages
    )

    return response.choices[0].message.content
```

#### `apps/api/src/api/shared/search.py` (Elasticsearch)

```python
"""
search.py - Elasticsearch integration

Autor: Homero Thompson del Lago del Terror
"""

from elasticsearch import AsyncElasticsearch

es = AsyncElasticsearch(["http://elasticsearch:9200"])

async def index_document(doc_id: int, title: str, content: str):
    """Index document in Elasticsearch."""
    await es.index(
        index="documents",
        id=doc_id,
        document={
            "title": title,
            "content": content,
            "indexed_at": datetime.now()
        }
    )

async def search_documents(query: str, limit: int = 10):
    """Full-text search with fuzzy matching."""
    response = await es.search(
        index="documents",
        body={
            "query": {
                "multi_match": {
                    "query": query,
                    "fields": ["title^2", "content"],
                    "fuzziness": "AUTO"
                }
            },
            "size": limit,
            "highlight": {
                "fields": {
                    "content": {}
                }
            }
        }
    )

    return [
        {
            "id": hit["_id"],
            "title": hit["_source"]["title"],
            "content": hit["_source"]["content"],
            "score": hit["_score"],
            "highlight": hit.get("highlight", {}).get("content", [])
        }
        for hit in response["hits"]["hits"]
    ]
```

#### `apps/web/lib/graphql/queries.ts` (Apollo Client)

```typescript
import { gql } from '@apollo/client';

export const SEMANTIC_SEARCH = gql`
  query SemanticSearch($query: String!, $limit: Int = 10) {
    semanticSearch(query: $query, limit: $limit) {
      id
      title
      content
      similarityScore
    }
  }
`;

export const FULL_TEXT_SEARCH = gql`
  query FullTextSearch($query: String!) {
    fullTextSearch(query: $query) {
      id
      title
      content
    }
  }
`;

export const INDEX_DOCUMENT = gql`
  mutation IndexDocument($title: String!, $content: String!) {
    indexDocument(title: $title, content: $content) {
      id
      title
    }
  }
`;

// Usage in component
export function SearchComponent() {
  const [semanticSearch, { data, loading }] = useLazyQuery(SEMANTIC_SEARCH);

  const handleSearch = (query: string) => {
    semanticSearch({ variables: { query } });
  };

  return (
    <div>
      <input onChange={(e) => handleSearch(e.target.value)} />
      {loading && <p>Searching...</p>}
      {data?.semanticSearch.map(doc => (
        <div key={doc.id}>
          <h3>{doc.title}</h3>
          <p>{doc.content}</p>
          <small>Similarity: {doc.similarityScore}</small>
        </div>
      ))}
    </div>
  );
}
```

---

## Example 4: Edge-First Global App

### Stack Selection

```yaml
architecture_pattern: monolith
backend: fastapi
frontend_platforms: [web]
web_framework: nextjs
api_style: [rest]
primary_database: supabase
caching: redis
deployment_strategy: edge
```

### Generated Project Structure

```
global-app/
├── apps/
│   ├── web/                      # Next.js with Edge Runtime
│   │   ├── app/
│   │   │   ├── api/              # Edge API routes
│   │   │   │   ├── auth/
│   │   │   │   │   └── route.ts  # Edge function
│   │   │   │   └── users/
│   │   │   │       └── route.ts
│   │   │   ├── (marketing)/
│   │   │   └── (app)/
│   │   ├── middleware.ts         # Edge middleware
│   │   └── package.json
│   │
│   └── api/                      # Origin server (non-edge)
│       └── ...
│
└── vercel.json
```

### Key Generated Files

#### `apps/web/app/api/auth/route.ts` (Edge API Route)

```typescript
/**
 * Edge API route - Auth
 * Runs on Vercel Edge (deployed globally)
 */

import { NextRequest, NextResponse } from 'next/server';
import { Redis } from '@upstash/redis';

// Edge-compatible Redis (Upstash)
const redis = new Redis({
  url: process.env.UPSTASH_REDIS_URL!,
  token: process.env.UPSTASH_REDIS_TOKEN!,
});

export const runtime = 'edge'; // Edge runtime

export async function POST(request: NextRequest) {
  const { email, password } = await request.json();

  // Validate credentials (call origin server if needed)
  const user = await validateUser(email, password);

  if (!user) {
    return NextResponse.json({ error: 'Invalid credentials' }, { status: 401 });
  }

  // Generate token
  const token = await generateToken(user);

  // Cache in Redis (edge-compatible)
  await redis.setex(`session:${token}`, 3600, JSON.stringify(user));

  // Return with cookie
  const response = NextResponse.json({ user });
  response.cookies.set('token', token, {
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: 3600,
  });

  return response;
}
```

#### `apps/web/middleware.ts` (Edge Middleware)

```typescript
/**
 * Edge Middleware - Auth, Geo-routing, A/B testing
 * Runs on EVERY request at the edge (<50ms globally)
 */

import { NextRequest, NextResponse } from 'next/server';
import { Redis } from '@upstash/redis';

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_URL!,
  token: process.env.UPSTASH_REDIS_TOKEN!,
});

export async function middleware(request: NextRequest) {
  // 1. Auth check
  const token = request.cookies.get('token')?.value;

  if (request.nextUrl.pathname.startsWith('/app')) {
    if (!token) {
      return NextResponse.redirect(new URL('/login', request.url));
    }

    // Validate token from Redis (cached session)
    const session = await redis.get(`session:${token}`);
    if (!session) {
      return NextResponse.redirect(new URL('/login', request.url));
    }
  }

  // 2. Geo-routing (redirect based on country)
  const country = request.geo?.country || 'US';

  if (request.nextUrl.pathname === '/' && country === 'ES') {
    return NextResponse.rewrite(new URL('/es', request.url));
  }

  // 3. A/B testing
  if (request.nextUrl.pathname === '/pricing') {
    const variant = Math.random() > 0.5 ? 'a' : 'b';
    const response = NextResponse.next();
    response.cookies.set('ab_test_pricing', variant);
    return response;
  }

  // 4. Rate limiting
  const ip = request.ip || 'unknown';
  const rateLimitKey = `rate_limit:${ip}`;
  const requests = await redis.incr(rateLimitKey);

  if (requests === 1) {
    await redis.expire(rateLimitKey, 60); // 60 second window
  }

  if (requests > 100) {
    return new NextResponse('Too many requests', { status: 429 });
  }

  return NextResponse.next();
}

export const config = {
  matcher: [
    '/((?!_next/static|_next/image|favicon.ico).*)',
  ],
};
```

---

## Summary: Code Generation Matrix

| Stack Choice | Generated Files | Key Features |
|--------------|----------------|--------------|
| **FastAPI** | `main.py`, `routes/`, `models/`, `services/` | Async, Pydantic, SQLModel |
| **Go + Fiber** | `main.go`, `internal/`, `proto/` | High perf, gRPC |
| **Next.js** | `app/`, `components/`, `lib/` | App Router, Server Components |
| **React Native** | `src/screens/`, `navigation/`, Expo config | 95% code sharing |
| **GraphQL** | `schema.py`, `queries.ts`, Apollo setup | Type-safe queries |
| **Redis** | `cache.py`, decorators | Sessions, rate limiting |
| **pgvector** | `embeddings.py`, semantic search | RAG, similarity search |
| **Elasticsearch** | `search.py`, indexing | Full-text, fuzzy search |
| **Edge** | `middleware.ts`, edge API routes | <50ms globally |
| **Microservices** | Per-service repos, proto, K8s manifests | Distributed, scalable |

---

**All examples include**:
- ✅ Docker/docker-compose for local development
- ✅ Tests (pytest, Jest)
- ✅ CI/CD (GitHub Actions)
- ✅ Type safety (Pydantic, TypeScript)
- ✅ Error handling
- ✅ Logging
- ✅ Documentation (OpenAPI, Storybook)

**Author**: Homero Thompson del Lago del Terror
**Generated by**: Enhanced Phase 3 wizard
