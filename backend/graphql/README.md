# GraphQL Backend Templates

> Production-ready GraphQL API templates for NestJS and FastAPI
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for building GraphQL APIs with:

### NestJS (TypeScript) - Code-First
- **Framework**: NestJS with Apollo Server
- **Schema**: Code-first with decorators
- **ORM**: Prisma ORM
- **Auth**: JWT + Guards
- **Validation**: class-validator
- **Testing**: Jest

### FastAPI (Python) - Code-First
- **Framework**: FastAPI with Strawberry GraphQL
- **Schema**: Code-first with Python types
- **ORM**: SQLModel (async)
- **Auth**: JWT dependencies
- **Validation**: Pydantic
- **Testing**: pytest

### Client Setup
- **Apollo Client**: React hooks
- **graphql-request**: Lightweight option
- **Code Generation**: GraphQL Codegen

---

## Directory Structure

```
backend/graphql/
├── nestjs/                     # NestJS + Apollo
│   ├── src/
│   │   ├── auth/
│   │   │   ├── auth.module.ts.tmpl
│   │   │   ├── auth.resolver.ts.tmpl
│   │   │   ├── jwt.strategy.ts.tmpl
│   │   │   └── gql-auth.guard.ts.tmpl
│   │   ├── users/
│   │   │   ├── users.module.ts.tmpl
│   │   │   ├── users.resolver.ts.tmpl
│   │   │   ├── users.service.ts.tmpl
│   │   │   └── user.entity.ts.tmpl
│   │   ├── common/
│   │   │   ├── scalars/
│   │   │   │   └── datetime.scalar.ts.tmpl
│   │   │   └── decorators/
│   │   │       └── current-user.decorator.ts.tmpl
│   │   ├── app.module.ts.tmpl
│   │   └── main.ts.tmpl
│   ├── schema.gql.tmpl         # Generated schema
│   ├── package.json.tmpl
│   └── tsconfig.json.tmpl
│
├── fastapi/                    # FastAPI + Strawberry
│   ├── src/
│   │   ├── graphql/
│   │   │   ├── schema.py.tmpl
│   │   │   ├── context.py.tmpl
│   │   │   └── scalars.py.tmpl
│   │   ├── resolvers/
│   │   │   ├── auth.py.tmpl
│   │   │   └── users.py.tmpl
│   │   ├── types/
│   │   │   ├── user.py.tmpl
│   │   │   └── auth.py.tmpl
│   │   └── main.py.tmpl
│   ├── pyproject.toml.tmpl
│   └── Dockerfile.tmpl
│
├── client/                     # Client integrations
│   ├── apollo/
│   │   ├── apollo-client.ts.tmpl
│   │   └── hooks.ts.tmpl
│   ├── graphql-request/
│   │   └── client.ts.tmpl
│   └── codegen/
│       ├── codegen.yml.tmpl
│       └── generated/
│
├── schema/                     # Shared schemas
│   └── schema.graphql.tmpl
│
└── docker-compose.graphql.yml.tmpl
```

---

## Quick Start

### NestJS + Apollo Server

#### 1. Setup Project

```bash
# Copy templates
mkdir -p apps/api-graphql
cp -r backend/graphql/nestjs/* apps/api-graphql/

# Install dependencies
cd apps/api-graphql
npm install

# Install NestJS CLI
npm install -g @nestjs/cli
```

#### 2. Configure

```bash
# .env
cat > .env <<EOF
PORT=4000
DATABASE_URL=postgresql://user:pass@localhost:5432/myapp
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=7d
EOF
```

#### 3. Run

```bash
# Development
npm run start:dev

# GraphQL Playground
open http://localhost:4000/graphql
```

### FastAPI + Strawberry

#### 1. Setup Project

```bash
# Copy templates
mkdir -p apps/api-graphql
cp -r backend/graphql/fastapi/* apps/api-graphql/

# Install with uv
cd apps/api-graphql
uv add fastapi strawberry-graphql sqlmodel uvicorn[standard]
```

#### 2. Configure

```bash
# .env
cat > .env <<EOF
DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/myapp
JWT_SECRET=your-secret-key
JWT_ALGORITHM=HS256
EOF
```

#### 3. Run

```bash
# Development
uv run uvicorn src.main:app --reload

# GraphiQL
open http://localhost:8000/graphql
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-graphql-api` |
| `{{AUTHOR}}` | Project author | `Your Name` |
| `{{DATABASE_URL}}` | Database connection | `postgresql://...` |
| `{{JWT_SECRET}}` | JWT secret key | `random-secret` |
| `{{PORT}}` | Server port | `4000` |

---

## NestJS Implementation

### Schema Definition (Code-First)

```typescript
// user.entity.ts
@ObjectType()
export class User {
  @Field(() => ID)
  id: string;

  @Field()
  email: string;

  @Field()
  name: string;

  @Field(() => Date)
  createdAt: Date;
}

@InputType()
export class CreateUserInput {
  @Field()
  @IsEmail()
  email: string;

  @Field()
  @MinLength(8)
  password: string;

  @Field()
  name: string;
}
```

### Resolver

```typescript
// users.resolver.ts
@Resolver(() => User)
export class UsersResolver {
  constructor(private usersService: UsersService) {}

  @Query(() => [User])
  @UseGuards(GqlAuthGuard)
  async users(): Promise<User[]> {
    return this.usersService.findAll();
  }

  @Query(() => User)
  async user(@Args('id', { type: () => ID }) id: string): Promise<User> {
    return this.usersService.findOne(id);
  }

  @Mutation(() => User)
  async createUser(@Args('input') input: CreateUserInput): Promise<User> {
    return this.usersService.create(input);
  }

  @ResolveField(() => [Post])
  async posts(@Parent() user: User) {
    return this.postsService.findByUserId(user.id);
  }
}
```

### Authentication Guard

```typescript
// gql-auth.guard.ts
@Injectable()
export class GqlAuthGuard extends AuthGuard('jwt') {
  getRequest(context: ExecutionContext) {
    const ctx = GqlExecutionContext.create(context);
    return ctx.getContext().req;
  }
}

// Usage with decorator
@Query(() => User)
@UseGuards(GqlAuthGuard)
async me(@CurrentUser() user: User): Promise<User> {
  return user;
}
```

### Apollo Server Config

```typescript
// app.module.ts
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  autoSchemaFile: 'schema.gql',
  sortSchema: true,
  playground: process.env.NODE_ENV !== 'production',
  context: ({ req }) => ({ req }),
  formatError: (error) => {
    // Custom error formatting
    return error;
  },
}),
```

---

## FastAPI Implementation

### Schema Definition (Code-First)

```python
# types/user.py
import strawberry
from datetime import datetime

@strawberry.type
class User:
    id: int
    email: str
    name: str
    created_at: datetime

@strawberry.input
class CreateUserInput:
    email: str
    password: str
    name: str
```

### Resolver

```python
# resolvers/users.py
@strawberry.type
class Query:
    @strawberry.field
    async def users(self, info: Info) -> list[User]:
        async with get_session() as session:
            result = await session.exec(select(UserModel))
            return [User.from_model(u) for u in result.all()]

    @strawberry.field
    async def user(self, info: Info, id: int) -> User:
        async with get_session() as session:
            user = await session.get(UserModel, id)
            if not user:
                raise NotFoundError("User not found")
            return User.from_model(user)

@strawberry.type
class Mutation:
    @strawberry.mutation
    async def create_user(self, input: CreateUserInput) -> User:
        # Validate and create user
        user = await create_user_service(input)
        return User.from_model(user)
```

### Authentication

```python
# context.py
async def get_context(
    request: Request,
    response: Response,
) -> GraphQLContext:
    token = request.headers.get("Authorization", "").replace("Bearer ", "")
    user = None
    if token:
        user = await verify_token(token)
    return GraphQLContext(user=user, request=request)

# Protected resolver
@strawberry.field
async def me(self, info: Info) -> User:
    if not info.context.user:
        raise AuthenticationError("Not authenticated")
    return info.context.user
```

### Strawberry Setup

```python
# main.py
from strawberry.fastapi import GraphQLRouter

schema = strawberry.Schema(query=Query, mutation=Mutation)

graphql_app = GraphQLRouter(
    schema,
    context_getter=get_context,
    graphiql=True,
)

app = FastAPI()
app.include_router(graphql_app, prefix="/graphql")
```

---

## Client Integration

### Apollo Client (React)

```typescript
// apollo-client.ts
import { ApolloClient, InMemoryCache, createHttpLink } from '@apollo/client';
import { setContext } from '@apollo/client/link/context';

const httpLink = createHttpLink({
  uri: 'http://localhost:4000/graphql',
});

const authLink = setContext((_, { headers }) => {
  const token = localStorage.getItem('token');
  return {
    headers: {
      ...headers,
      authorization: token ? `Bearer ${token}` : "",
    }
  };
});

export const client = new ApolloClient({
  link: authLink.concat(httpLink),
  cache: new InMemoryCache(),
});
```

### React Hooks

```typescript
// hooks.ts
import { gql, useQuery, useMutation } from '@apollo/client';

const GET_USERS = gql`
  query GetUsers {
    users {
      id
      email
      name
    }
  }
`;

export function useUsers() {
  return useQuery(GET_USERS);
}

const CREATE_USER = gql`
  mutation CreateUser($input: CreateUserInput!) {
    createUser(input: $input) {
      id
      email
      name
    }
  }
`;

export function useCreateUser() {
  return useMutation(CREATE_USER);
}
```

### graphql-request (Lightweight)

```typescript
// client.ts
import { GraphQLClient } from 'graphql-request';

const client = new GraphQLClient('http://localhost:4000/graphql', {
  headers: {
    authorization: `Bearer ${token}`,
  },
});

// Usage
const data = await client.request(
  gql`
    query GetUsers {
      users {
        id
        email
      }
    }
  `
);
```

---

## Code Generation

### GraphQL Codegen

```yaml
# codegen.yml
schema: http://localhost:4000/graphql
documents: 'src/**/*.graphql'
generates:
  src/generated/graphql.ts:
    plugins:
      - typescript
      - typescript-operations
      - typescript-react-apollo
    config:
      withHooks: true
      withComponent: false
```

### Usage

```bash
# Install
npm install -D @graphql-codegen/cli @graphql-codegen/typescript

# Generate
npm run codegen

# Use generated types
import { useGetUsersQuery, User } from './generated/graphql';

const { data, loading } = useGetUsersQuery();
```

---

## Features

### NestJS Features

- **Code-first schema**: TypeScript decorators
- **Automatic validation**: class-validator
- **Dependency injection**: NestJS DI
- **Module system**: Organized architecture
- **Testing**: Jest integration
- **Subscriptions**: WebSocket support

### FastAPI Features

- **Code-first schema**: Python type hints
- **Automatic validation**: Pydantic
- **Async by default**: asyncio support
- **Fast execution**: Strawberry performance
- **Type safety**: Full typing
- **GraphiQL**: Built-in explorer

---

## Example Queries

### Authentication

```graphql
# Register
mutation {
  register(input: {
    email: "user@example.com"
    password: "password123"
    name: "John Doe"
  }) {
    accessToken
    user {
      id
      email
      name
    }
  }
}

# Login
mutation {
  login(email: "user@example.com", password: "password123") {
    accessToken
    refreshToken
  }
}
```

### Queries

```graphql
# Get all users
query {
  users {
    id
    email
    name
    createdAt
  }
}

# Get user with relations
query {
  user(id: "1") {
    id
    name
    posts {
      id
      title
    }
    profile {
      bio
      avatar
    }
  }
}

# Paginated query
query {
  users(page: 1, limit: 10) {
    items {
      id
      name
    }
    pageInfo {
      hasNextPage
      totalPages
    }
  }
}
```

### Mutations

```graphql
# Create user
mutation {
  createUser(input: {
    email: "new@example.com"
    password: "secure123"
    name: "Jane"
  }) {
    id
    email
  }
}

# Update user
mutation {
  updateUser(id: "1", input: { name: "Updated Name" }) {
    id
    name
  }
}

# Delete user
mutation {
  deleteUser(id: "1")
}
```

### Subscriptions (NestJS)

```graphql
subscription {
  userCreated {
    id
    email
    name
  }
}
```

---

## Best Practices

### Schema Design

- Use meaningful names
- Consistent naming conventions (camelCase)
- Proper input/output types
- Pagination for lists
- Error handling

### Performance

- **DataLoader**: Batch and cache database queries
- **Complexity Analysis**: Limit query depth
- **Persisted Queries**: Whitelist queries
- **Caching**: Redis for field-level caching

### Security

- **Authentication**: JWT on context
- **Authorization**: Field-level guards
- **Query Depth Limiting**: Prevent deep queries
- **Query Cost Analysis**: Rate limiting
- **Input Validation**: Sanitize all inputs

---

## Testing

### NestJS Tests

```typescript
describe('UsersResolver', () => {
  let resolver: UsersResolver;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [UsersResolver, UsersService],
    }).compile();

    resolver = module.get<UsersResolver>(UsersResolver);
  });

  it('should return users', async () => {
    const users = await resolver.users();
    expect(users).toBeDefined();
  });
});
```

### FastAPI Tests

```python
def test_query_users(client: TestClient):
    response = client.post(
        "/graphql",
        json={
            "query": "{ users { id email } }"
        }
    )
    assert response.status_code == 200
    data = response.json()
    assert "data" in data
```

---

## Resources

### Documentation

- [NestJS GraphQL](https://docs.nestjs.com/graphql/quick-start)
- [Apollo Server](https://www.apollographql.com/docs/apollo-server/)
- [Strawberry GraphQL](https://strawberry.rocks/)
- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
- [Apollo Client](https://www.apollographql.com/docs/react/)

### Tools

- [GraphQL Playground](https://github.com/graphql/graphql-playground)
- [GraphQL Voyager](https://apis.guru/graphql-voyager/)
- [GraphQL Inspector](https://graphql-inspector.com/)

---

## Support

For issues or questions:
1. Check the GraphQL documentation
2. Review framework-specific guides
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
