# MongoDB Database Templates

Author: Homero Thompson del Lago del Terror
Project: nuevo-proyecto-scaffold

---

## Overview

Production-ready MongoDB templates for TypeScript/Node.js (NestJS) and Python (FastAPI) projects with:

- ✅ Async-first architecture (Motor + Beanie for Python, Mongoose for TypeScript)
- ✅ Repository pattern with base CRUD operations
- ✅ Soft delete support
- ✅ Comprehensive indexing strategies
- ✅ Migration system
- ✅ Docker Compose setup with Mongo Express
- ✅ Connection pooling and retry logic
- ✅ Type-safe schemas (Pydantic + Beanie / Mongoose)

---

## File Structure

```
mongodb/
├── TypeScript/Node.js
│   ├── client.ts.tmpl                     # Mongoose client with singleton pattern
│   ├── connection.ts.tmpl                 # Connection manager with retry logic
│   ├── .env.example.tmpl                  # Environment variables template
│   ├── schemas/
│   │   ├── base.schema.ts.tmpl           # Base schema with timestamps + soft delete
│   │   ├── user.schema.ts.tmpl           # User schema with validations
│   │   └── index.ts.tmpl                 # Schema exports
│   ├── repositories/
│   │   ├── base.repository.ts.tmpl       # Generic repository pattern
│   │   └── user.repository.ts.tmpl       # User-specific operations
│   ├── hooks/
│   │   └── use-mongodb.ts.tmpl           # React hooks for MongoDB API
│   ├── indexes/
│   │   └── user.indexes.ts.tmpl          # Index definitions + management
│   └── migrations/
│       └── 001-initial.ts.tmpl           # Initial migration + runner
│
├── Python/FastAPI
│   ├── motor_client.py.tmpl              # Motor client with Beanie
│   ├── models/
│   │   ├── base.py.tmpl                  # Base document with mixins
│   │   └── user.py.tmpl                  # User model with Beanie
│   └── repositories/
│       └── user_repository.py.tmpl       # Async user repository
│
├── Docker
│   ├── docker-compose.mongodb.yml.tmpl   # MongoDB + Mongo Express
│   └── init-mongo.js.tmpl                # Initialization script
│
└── README.md                              # This file
```

---

## Quick Start

### TypeScript/Node.js

#### 1. Install Dependencies

```bash
npm install mongoose bcryptjs
npm install -D @types/mongoose @types/bcryptjs
```

#### 2. Configure Environment

```bash
cp .env.example .env
# Edit MONGODB_URI in .env
```

#### 3. Initialize Connection

```typescript
import { createConnectionManager, setupGracefulShutdown } from './connection';

const connectionManager = createConnectionManager({
  uri: process.env.MONGODB_URI!,
});

await connectionManager.connect();
setupGracefulShutdown();
```

#### 4. Use Repository

```typescript
import { userRepository } from './repositories/user.repository';

// Create user
const user = await userRepository.create({
  email: 'user@example.com',
  username: 'johndoe',
  password: 'securepassword',
});

// Find by email
const found = await userRepository.findByEmail('user@example.com');

// Paginated query
const results = await userRepository.findWithPagination(
  { isActive: true },
  { page: 1, limit: 20 }
);
```

#### 5. Run Migrations

```typescript
import { migrationRunner } from './migrations/001-initial';

// Run all pending migrations
await migrationRunner.migrate();

// Rollback last migration
await migrationRunner.rollback(1);
```

---

### Python/FastAPI

#### 1. Install Dependencies

```bash
uv add motor beanie pydantic-settings bcrypt
```

#### 2. Configure Environment

```bash
cp .env.example .env
# Edit MONGODB_URI in .env
```

#### 3. Initialize Connection

```python
from motor_client import connect_to_mongodb, setup_indexes

await connect_to_mongodb(
    uri=settings.mongodb_uri,
    database=settings.mongodb_database
)
await setup_indexes()
```

#### 4. Use Repository

```python
from repositories.user_repository import user_repository

# Create user
user = await user_repository.create(
    email='user@example.com',
    username='johndoe',
    password='securepassword'
)

# Find by email
found = await user_repository.find_by_email('user@example.com')

# Get stats
stats = await user_repository.get_stats()
```

#### 5. Use in FastAPI

```python
from fastapi import FastAPI, Depends
from motor_client import mongodb_client, connect_to_mongodb, disconnect_from_mongodb

app = FastAPI()

@app.on_event("startup")
async def startup():
    await connect_to_mongodb(settings.mongodb_uri, settings.mongodb_database)

@app.on_event("shutdown")
async def shutdown():
    await disconnect_from_mongodb()

@app.get("/users/{user_id}")
async def get_user(user_id: str):
    user = await user_repository.find_by_id(user_id)
    return user
```

---

### Docker Setup

#### 1. Start MongoDB Stack

```bash
docker-compose -f docker-compose.mongodb.yml up -d
```

#### 2. Access Mongo Express

Open browser: http://localhost:8081
- Username: `admin`
- Password: `express_password`

#### 3. Connection String

```
mongodb://admin:admin_password@localhost:27017/{{PROJECT_NAME}}?authSource=admin
```

---

## Features

### Base Schema/Model

**TypeScript:**
- Timestamps (createdAt, updatedAt)
- Soft delete (deletedAt, isDeleted)
- Virtual fields (id transformation)
- Pre-query middleware to exclude soft-deleted

**Python:**
- Beanie Document with mixins
- Automatic timestamp management
- Soft delete methods
- JSON serialization with ISO dates

### User Schema/Model

**Fields:**
- Email (unique, validated)
- Username (unique, alphanumeric with `-_`)
- Password (hashed with bcrypt)
- Names (first, last)
- Avatar URL
- Role (user, admin, moderator)
- Activation status
- Email verification
- Password reset tokens
- Last login timestamp
- Custom metadata

**Indexes:**
- Unique: email, username
- Single: role, isActive, isEmailVerified, createdAt
- Compound: (isActive, role), (isEmailVerified, isActive)
- Text: username, email, firstName, lastName
- TTL: Auto-delete unverified users after 30 days

### Repository Pattern

**Base Operations:**
- create, createMany
- findById, findOne, find
- findWithPagination (TypeScript) / find_all with skip/limit (Python)
- update, updateOne, updateMany
- delete, deleteOne, deleteMany (soft or hard)
- count, exists
- aggregate

**User-Specific:**
- findByEmail, findByUsername
- findByRole, findActiveUsers
- updateLastLogin, setEmailVerified
- searchUsers (regex on multiple fields)
- getUserStats (totals by role, active, verified)

### Migration System (TypeScript)

```typescript
const migration_001: Migration = {
  version: 1,
  name: 'initial_setup',
  async up() { /* ... */ },
  async down() { /* ... */ }
};
```

**Commands:**
- `migrationRunner.migrate()` - Run pending
- `migrationRunner.rollback(n)` - Rollback last n
- `migrationRunner.reset()` - Reset all

### React Hooks (TypeScript)

```typescript
// Query
const { data, loading, error, refetch } = useMongoDBQuery('/api/users', {
  autoFetch: true
});

// Mutation
const { execute: createUser } = useMongoDBMutation('/api/users', 'POST');

// Pagination
const { data, page, totalPages, nextPage, prevPage } = useMongoDBPagination(
  '/api/users',
  1,
  10
);
```

---

## Environment Variables

```bash
# Required
MONGODB_URI=mongodb://localhost:27017/myapp

# Optional
MONGODB_MAX_POOL_SIZE=10
MONGODB_MIN_POOL_SIZE=2
MONGODB_SERVER_SELECTION_TIMEOUT_MS=5000
MONGODB_DATABASE=myapp
```

**Production formats:**
```bash
# With auth
mongodb://user:password@host:27017/database?authSource=admin

# Atlas
mongodb+srv://user:password@cluster.mongodb.net/database?retryWrites=true&w=majority

# Replica set
mongodb://host1:27017,host2:27017,host3:27017/database?replicaSet=rs0
```

---

## Best Practices

### Indexing

1. **Most selective fields first** in compound indexes
2. **Equality before range** filters
3. **Sort fields last**
4. Use `explain()` to verify index usage
5. Remove unused indexes
6. Monitor with `listUserIndexes()`

### Queries

1. **Use projections** to limit returned fields
2. **Avoid $where** (uses JavaScript, slow)
3. **Use covered queries** when possible
4. **Paginate** large result sets
5. **Batch operations** with insertMany/updateMany

### Security

1. **Never expose passwords** in API responses (use `select: false`)
2. **Hash passwords** with bcrypt (10+ rounds)
3. **Validate user input** with schema validators
4. **Use TTL indexes** to auto-delete expired data
5. **Enable authentication** in production

### Performance

1. **Connection pooling**: Set appropriate min/max pool sizes
2. **Retry logic**: Handle transient network failures
3. **Soft delete**: Faster than hard delete, allows recovery
4. **Lean queries**: Use `.lean()` in Mongoose for read-only data
5. **Bulk operations**: Use insertMany, updateMany for batches

---

## Troubleshooting

### Connection Issues

```bash
# Test connection
mongosh "mongodb://localhost:27017"

# Check Docker logs
docker logs {{PROJECT_NAME}}_mongodb

# Verify network
docker network inspect mongodb_network
```

### Index Problems

```typescript
// List indexes
await User.collection.getIndexes();

// Drop all indexes
await User.collection.dropIndexes();

// Recreate
await createUserIndexes();
```

### Migration Issues

```typescript
// Check applied migrations
const db = mongoose.connection.db;
const migrations = await db.collection('migrations').find().toArray();

// Manual rollback
await db.collection('migrations').deleteOne({ version: 1 });
```

---

## Production Checklist

- [ ] Enable authentication (`--auth`)
- [ ] Use replica set for high availability
- [ ] Configure backup strategy
- [ ] Monitor slow queries
- [ ] Set up alerts for connection pool exhaustion
- [ ] Use connection string with `retryWrites=true`
- [ ] Enable TLS/SSL
- [ ] Implement rate limiting
- [ ] Use read/write concerns appropriately
- [ ] Regular index optimization

---

## References

- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [Beanie Documentation](https://beanie-odm.dev/)
- [Motor Documentation](https://motor.readthedocs.io/)
- [MongoDB Best Practices](https://www.mongodb.com/docs/manual/administration/production-notes/)
- [Indexing Strategies](https://www.mongodb.com/docs/manual/applications/indexes/)

---

## Changelog

### 2026-01-19
- Initial template creation
- TypeScript templates with Mongoose
- Python templates with Motor + Beanie
- Docker Compose setup
- Migration system
- React hooks for API integration
