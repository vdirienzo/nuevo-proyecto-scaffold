# Redis/Dragonfly Cache Templates

Templates para integración de Redis y Dragonfly en proyectos Node.js/TypeScript y Python/FastAPI.

## Estructura

```
redis/
├── typescript/          # Templates TypeScript/Node.js
│   ├── client.ts.tmpl
│   ├── config.ts.tmpl
│   ├── cache/
│   │   ├── cache.service.ts.tmpl
│   │   ├── cache.decorator.ts.tmpl
│   │   └── cache.module.ts.tmpl
│   ├── session/
│   │   ├── session.store.ts.tmpl
│   │   └── session.middleware.ts.tmpl
│   ├── queue/
│   │   ├── queue.service.ts.tmpl
│   │   └── queue.processor.ts.tmpl
│   ├── pubsub/
│   │   ├── publisher.ts.tmpl
│   │   └── subscriber.ts.tmpl
│   └── rate-limit/
│       └── rate-limiter.ts.tmpl
├── python/              # Templates Python/FastAPI
│   ├── redis_client.py.tmpl
│   ├── cache_service.py.tmpl
│   ├── rate_limiter.py.tmpl
│   └── session_backend.py.tmpl
├── docker/              # Docker configurations
│   ├── docker-compose.redis.yml.tmpl
│   ├── docker-compose.dragonfly.yml.tmpl
│   └── redis.conf.tmpl
├── .env.example.tmpl
└── README.md
```

## TypeScript/Node.js

### Core

#### Redis Client (`typescript/client.ts.tmpl`)
Singleton Redis client con manejo de reconexión automática.

**Dependencias:**
```bash
npm install ioredis
# or
pnpm add ioredis
```

**Uso:**
```typescript
import { redisClient } from './client';

await redisClient.set('key', 'value');
const value = await redisClient.get('key');
```

#### Config (`typescript/config.ts.tmpl`)
Configuración centralizada de Redis con validación Zod.

### Cache

#### Cache Service (`typescript/cache/cache.service.ts.tmpl`)
Servicio genérico de caché con operaciones comunes.

**Uso:**
```typescript
import { cacheService } from './cache/cache.service';

// Get or set
const user = await cacheService.getOrSet(
  'user:123',
  async () => await fetchUser(123),
  REDIS_TTL.LONG
);

// Delete pattern
await cacheService.deletePattern('user:*');
```

#### Cache Decorator (`typescript/cache/cache.decorator.ts.tmpl`)
Decoradores para cachear resultados de métodos.

**Uso:**
```typescript
class UserService {
  @Cacheable({ ttl: REDIS_TTL.LONG, prefix: 'user:' })
  async getUser(id: string) {
    return await db.getUser(id);
  }

  @CacheEvict({ patterns: ['user:*'] })
  async updateUser(id: string, data: any) {
    return await db.updateUser(id, data);
  }
}
```

#### Cache Module (`typescript/cache/cache.module.ts.tmpl`)
Módulo NestJS para integración de caché.

**Uso:**
```typescript
@Module({
  imports: [
    CacheModule.forRoot({
      ttl: REDIS_TTL.MEDIUM,
      prefix: 'myapp:',
      isGlobal: true,
    }),
  ],
})
export class AppModule {}
```

### Session

#### Session Store (`typescript/session/session.store.ts.tmpl`)
Almacenamiento de sesiones en Redis.

**Uso:**
```typescript
import { sessionStore } from './session/session.store';

// Create session
await sessionStore.create('session-id', {
  userId: '123',
  email: 'user@example.com',
  roles: ['user'],
});

// Get session
const session = await sessionStore.get('session-id');
```

#### Session Middleware (`typescript/session/session.middleware.ts.tmpl`)
Middleware Express para manejo de sesiones.

**Uso:**
```typescript
import { sessionMiddleware } from './session/session.middleware';

app.use(sessionMiddleware({
  cookieName: 'sessionId',
  cookieMaxAge: 7 * 24 * 60 * 60 * 1000, // 7 days
}));
```

### Queue

#### Queue Service (`typescript/queue/queue.service.ts.tmpl`)
Servicio de colas con BullMQ.

**Dependencias:**
```bash
npm install bullmq
```

**Uso:**
```typescript
import { queueService, QUEUE_NAMES } from './queue/queue.service';

// Add job
await queueService.addJob(
  QUEUE_NAMES.EMAIL,
  'send-welcome',
  { to: 'user@example.com', subject: 'Welcome!' }
);
```

#### Queue Processor (`typescript/queue/queue.processor.ts.tmpl`)
Procesador de trabajos en cola.

**Uso:**
```typescript
import { queueProcessor } from './queue/queue.processor';

queueProcessor.registerProcessor('email', async (job) => {
  const { to, subject, body } = job.data;
  await sendEmail(to, subject, body);
}, { concurrency: 10 });
```

### Pub/Sub

#### Publisher (`typescript/pubsub/publisher.ts.tmpl`)
Publicador Redis Pub/Sub.

**Uso:**
```typescript
import { publisher, CHANNELS } from './pubsub/publisher';

await publisher.publish(CHANNELS.USER_EVENTS, {
  action: 'login',
  userId: '123',
});
```

#### Subscriber (`typescript/pubsub/subscriber.ts.tmpl`)
Suscriptor Redis Pub/Sub.

**Uso:**
```typescript
import { subscriber } from './pubsub/subscriber';

await subscriber.subscribe('user:events', (message) => {
  console.log('Event:', message);
});

// Pattern subscription
await subscriber.psubscribe('user:*', (message, channel) => {
  console.log(`From ${channel}:`, message);
});
```

### Rate Limiting

#### Rate Limiter (`typescript/rate-limit/rate-limiter.ts.tmpl`)
Limitador de tasa con Redis.

**Uso:**
```typescript
import { rateLimiters } from './rate-limit/rate-limiter';

// Apply to all routes
app.use(rateLimiters.standard());

// Apply to specific route
app.post('/api/login', rateLimiters.auth(), loginHandler);
```

## Python/FastAPI

### Redis Client (`python/redis_client.py.tmpl`)
Cliente Redis asíncrono con aioredis.

**Dependencias:**
```bash
pip install redis[asyncio]
# or with uv
uv add redis[asyncio]
```

**Uso:**
```python
from redis_client import get_redis

redis = await get_redis()
await redis.set('key', 'value')
value = await redis.get('key')
```

### Cache Service (`python/cache_service.py.tmpl`)
Servicio de caché con decorador.

**Uso:**
```python
from cache_service import cached, RedisTTL

@cached(key_builder=lambda user_id: f"user:{user_id}", ttl=RedisTTL.LONG)
async def get_user(user_id: str):
    return await db.get_user(user_id)
```

### Rate Limiter (`python/rate_limiter.py.tmpl`)
Middleware de rate limiting para FastAPI.

**Uso:**
```python
from rate_limiter import RateLimitPresets
from fastapi import Depends

@app.post("/api/login", dependencies=[Depends(RateLimitPresets.auth())])
async def login(credentials: LoginCredentials):
    return await authenticate(credentials)
```

### Session Backend (`python/session_backend.py.tmpl`)
Backend de sesiones con middleware FastAPI.

**Uso:**
```python
from session_backend import SessionMiddleware

app = FastAPI()
app.add_middleware(SessionMiddleware)

@app.get("/profile")
async def profile(session: SessionData = Depends(get_current_session)):
    return {"user_id": session.user_id}
```

## Docker

### Redis (`docker/docker-compose.redis.yml.tmpl`)
Stack completo de Redis con Redis Commander y Redis Insight.

**Uso:**
```bash
docker compose -f docker-compose.redis.yml up -d
```

**Servicios:**
- Redis (puerto 6379)
- Redis Commander (puerto 8081) - GUI simple
- Redis Insight (puerto 8001) - GUI moderna

### Dragonfly (`docker/docker-compose.dragonfly.yml.tmpl`)
Alternativa moderna a Redis (25x más rápido).

**Uso:**
```bash
docker compose -f docker-compose.dragonfly.yml up -d
```

**Características:**
- Compatible con API de Redis
- 25x más rápido
- Menor consumo de memoria
- Escalado vertical automático

### Redis Configuration (`docker/redis.conf.tmpl`)
Archivo de configuración Redis optimizado.

## Variables de Entorno

Ver `.env.example.tmpl` para todas las variables disponibles:

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_DB=0
REDIS_KEY_PREFIX=myapp:
REDIS_TTL=3600
```

## Decisión: Redis vs Dragonfly

### Usar Redis cuando:
- Necesitas ecosistema maduro con amplia adopción
- Requieres módulos Redis específicos (RedisJSON, RedisGraph)
- Tienes experiencia previa con Redis
- Necesitas clustering Redis nativo

### Usar Dragonfly cuando:
- Necesitas máximo rendimiento
- Quieres reducir costos de memoria
- No requieres módulos Redis específicos
- Buscas simplicidad operacional (menos configuración)

**Recomendación:** Para nuevos proyectos, Dragonfly ofrece mejor rendimiento con la misma API.

## Autor

{{AUTHOR}}

## Licencia

MIT
