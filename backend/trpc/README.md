# tRPC Backend Templates

> End-to-end typesafe APIs with tRPC and TypeScript
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for building fully type-safe APIs with tRPC:

- **Type Safety**: End-to-end TypeScript types
- **Framework Agnostic**: Works with Next.js, Express, Fastify
- **React Integration**: React Query hooks
- **Validation**: Zod schemas
- **Middleware**: Auth, logging, rate limiting
- **WebSockets**: Subscriptions support
- **Batching**: Request batching and caching
- **OpenAPI**: REST-like documentation

---

## Directory Structure

```
backend/trpc/
├── server/                         # Server setup
│   ├── context.ts.tmpl              # Context creation
│   ├── router.ts.tmpl               # Root router
│   ├── trpc.ts.tmpl                 # tRPC instance
│   └── routers/                     # Feature routers
│       ├── auth.ts.tmpl
│       ├── users.ts.tmpl
│       └── posts.ts.tmpl
│
├── client/                         # Client setup
│   ├── react.ts.tmpl                # React client
│   ├── vanilla.ts.tmpl              # Vanilla client
│   └── next.ts.tmpl                 # Next.js client
│
├── middleware/                     # Middleware
│   ├── auth.ts.tmpl
│   ├── logging.ts.tmpl
│   └── ratelimit.ts.tmpl
│
├── procedures/                     # Reusable procedures
│   ├── public.ts.tmpl
│   └── protected.ts.tmpl
│
└── adapters/                       # Framework adapters
    ├── express.ts.tmpl
    ├── nextjs.ts.tmpl
    └── fastify.ts.tmpl
```

---

## Quick Start

### 1. Server Setup

#### Install Dependencies

```bash
npm install @trpc/server @trpc/client @trpc/react-query @tanstack/react-query zod
```

#### Create tRPC Instance

```typescript
// server/trpc.ts
import { initTRPC, TRPCError } from '@trpc/server';
import { Context } from './context';
import superjson from 'superjson';

const t = initTRPC.context<Context>().create({
  transformer: superjson,
  errorFormatter({ shape, error }) {
    return {
      ...shape,
      data: {
        ...shape.data,
        zodError:
          error.cause instanceof ZodError ? error.cause.flatten() : null,
      },
    };
  },
});

export const router = t.router;
export const publicProcedure = t.procedure;
export const middleware = t.middleware;
```

#### Create Context

```typescript
// server/context.ts
import { inferAsyncReturnType } from '@trpc/server';
import { CreateNextContextOptions } from '@trpc/server/adapters/next';

export async function createContext(opts: CreateNextContextOptions) {
  const { req, res } = opts;

  // Get user from session/JWT
  const user = await getUserFromHeader(req.headers.authorization);

  return {
    req,
    res,
    user,
    prisma,
  };
}

export type Context = inferAsyncReturnType<typeof createContext>;
```

#### Create Router

```typescript
// server/router.ts
import { z } from 'zod';
import { router, publicProcedure } from './trpc';

export const appRouter = router({
  hello: publicProcedure
    .input(z.object({ name: z.string() }))
    .query(({ input }) => {
      return { greeting: `Hello ${input.name}!` };
    }),

  createUser: publicProcedure
    .input(z.object({
      email: z.string().email(),
      name: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      const user = await ctx.prisma.user.create({
        data: input,
      });
      return user;
    }),
});

export type AppRouter = typeof appRouter;
```

### 2. Next.js Integration

#### API Route

```typescript
// pages/api/trpc/[trpc].ts
import { createNextApiHandler } from '@trpc/server/adapters/next';
import { appRouter } from '@/server/router';
import { createContext } from '@/server/context';

export default createNextApiHandler({
  router: appRouter,
  createContext,
  onError({ error, type, path, input }) {
    console.error('tRPC Error:', { error, type, path, input });
  },
});
```

#### Client Setup

```typescript
// utils/trpc.ts
import { createTRPCNext } from '@trpc/next';
import { httpBatchLink } from '@trpc/client';
import type { AppRouter } from '@/server/router';
import superjson from 'superjson';

export const trpc = createTRPCNext<AppRouter>({
  config({ ctx }) {
    return {
      transformer: superjson,
      links: [
        httpBatchLink({
          url: '/api/trpc',
          headers() {
            if (ctx?.req) {
              return {
                ...ctx.req.headers,
                'x-ssr': '1',
              };
            }
            return {};
          },
        }),
      ],
    };
  },
  ssr: true,
});
```

#### Provider

```typescript
// pages/_app.tsx
import { trpc } from '@/utils/trpc';

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}

export default trpc.withTRPC(MyApp);
```

### 3. React Usage

```typescript
// components/UsersList.tsx
import { trpc } from '@/utils/trpc';

export function UsersList() {
  // Query with React Query hooks
  const { data, isLoading } = trpc.users.list.useQuery();

  // Mutation
  const createUser = trpc.users.create.useMutation({
    onSuccess() {
      // Invalidate and refetch
      utils.users.list.invalidate();
    },
  });

  const handleCreate = () => {
    createUser.mutate({
      email: 'user@example.com',
      name: 'John Doe',
    });
  };

  if (isLoading) return <div>Loading...</div>;

  return (
    <div>
      {data?.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
      <button onClick={handleCreate}>Create User</button>
    </div>
  );
}
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-trpc-app` |
| `{{AUTHOR}}` | Author name | `Your Name` |
| `{{DATABASE_URL}}` | Database URL | `postgresql://...` |
| `{{JWT_SECRET}}` | JWT secret | `your-secret` |

---

## Core Concepts

### Procedures

Three types of procedures:

```typescript
// Query - for fetching data
publicProcedure.query()

// Mutation - for modifying data
publicProcedure.mutation()

// Subscription - for real-time updates
publicProcedure.subscription()
```

### Input Validation (Zod)

```typescript
import { z } from 'zod';

const userSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
  age: z.number().int().positive().optional(),
});

router({
  createUser: publicProcedure
    .input(userSchema)
    .mutation(({ input }) => {
      // input is fully typed!
      return createUser(input);
    }),
});
```

### Middleware

```typescript
// Auth middleware
const isAuthed = middleware(async ({ ctx, next }) => {
  if (!ctx.user) {
    throw new TRPCError({ code: 'UNAUTHORIZED' });
  }
  return next({
    ctx: {
      user: ctx.user, // Type-safe user
    },
  });
});

// Protected procedure
const protectedProcedure = publicProcedure.use(isAuthed);

router({
  getMe: protectedProcedure.query(({ ctx }) => {
    return ctx.user; // Guaranteed to exist
  }),
});
```

---

## Feature Routers

### Auth Router

```typescript
// server/routers/auth.ts
import { router, publicProcedure } from '../trpc';
import { z } from 'zod';
import { TRPCError } from '@trpc/server';

export const authRouter = router({
  login: publicProcedure
    .input(z.object({
      email: z.string().email(),
      password: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      const user = await ctx.prisma.user.findUnique({
        where: { email: input.email },
      });

      if (!user || !await verifyPassword(input.password, user.password)) {
        throw new TRPCError({
          code: 'UNAUTHORIZED',
          message: 'Invalid credentials',
        });
      }

      const token = generateToken(user.id);
      return { token, user };
    }),

  register: publicProcedure
    .input(z.object({
      email: z.string().email(),
      password: z.string().min(8),
      name: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      const exists = await ctx.prisma.user.findUnique({
        where: { email: input.email },
      });

      if (exists) {
        throw new TRPCError({
          code: 'CONFLICT',
          message: 'User already exists',
        });
      }

      const user = await ctx.prisma.user.create({
        data: {
          ...input,
          password: await hashPassword(input.password),
        },
      });

      const token = generateToken(user.id);
      return { token, user };
    }),

  me: protectedProcedure.query(({ ctx }) => {
    return ctx.user;
  }),
});
```

### Users Router

```typescript
// server/routers/users.ts
export const usersRouter = router({
  list: protectedProcedure
    .input(z.object({
      limit: z.number().min(1).max(100).default(10),
      cursor: z.string().optional(),
    }))
    .query(async ({ input, ctx }) => {
      const users = await ctx.prisma.user.findMany({
        take: input.limit + 1,
        cursor: input.cursor ? { id: input.cursor } : undefined,
        orderBy: { createdAt: 'desc' },
      });

      let nextCursor: string | undefined;
      if (users.length > input.limit) {
        const nextItem = users.pop();
        nextCursor = nextItem!.id;
      }

      return { users, nextCursor };
    }),

  byId: protectedProcedure
    .input(z.object({ id: z.string() }))
    .query(async ({ input, ctx }) => {
      const user = await ctx.prisma.user.findUnique({
        where: { id: input.id },
      });

      if (!user) {
        throw new TRPCError({
          code: 'NOT_FOUND',
          message: 'User not found',
        });
      }

      return user;
    }),

  update: protectedProcedure
    .input(z.object({
      id: z.string(),
      name: z.string().optional(),
      email: z.string().email().optional(),
    }))
    .mutation(async ({ input, ctx }) => {
      return ctx.prisma.user.update({
        where: { id: input.id },
        data: input,
      });
    }),
});
```

### Merge Routers

```typescript
// server/router.ts
export const appRouter = router({
  auth: authRouter,
  users: usersRouter,
  posts: postsRouter,
});

// Usage in client:
// trpc.auth.login.useMutation()
// trpc.users.list.useQuery()
```

---

## Advanced Features

### Infinite Queries

```typescript
// Server
list: publicProcedure
  .input(z.object({
    limit: z.number().min(1).max(100).default(20),
    cursor: z.string().optional(),
  }))
  .query(({ input }) => {
    const items = getItems(input.cursor, input.limit);
    return {
      items,
      nextCursor: items[items.length - 1]?.id,
    };
  }),

// Client
const { data, fetchNextPage, hasNextPage } =
  trpc.users.list.useInfiniteQuery(
    { limit: 20 },
    {
      getNextPageParam: (lastPage) => lastPage.nextCursor,
    }
  );
```

### Subscriptions (WebSockets)

```typescript
// Server
import { observable } from '@trpc/server/observable';

onMessage: publicProcedure.subscription(() => {
  return observable<Message>((emit) => {
    const onMessage = (message: Message) => {
      emit.next(message);
    };

    eventEmitter.on('message', onMessage);

    return () => {
      eventEmitter.off('message', onMessage);
    };
  });
}),

// Client
trpc.onMessage.useSubscription(undefined, {
  onData(message) {
    console.log('New message:', message);
  },
});
```

### Server-Sent Events

```typescript
// Server
import { TRPCError } from '@trpc/server';

progress: publicProcedure
  .input(z.object({ taskId: z.string() }))
  .subscription(({ input }) => {
    return observable<number>((emit) => {
      let progress = 0;
      const interval = setInterval(() => {
        progress += 10;
        emit.next(progress);
        if (progress >= 100) {
          clearInterval(interval);
          emit.complete();
        }
      }, 1000);

      return () => clearInterval(interval);
    });
  }),
```

### Request Batching

```typescript
// Automatic batching
const client = createTRPCClient<AppRouter>({
  links: [
    httpBatchLink({
      url: '/api/trpc',
      maxBatchSize: 10, // Max 10 requests per batch
    }),
  ],
});

// Multiple calls batched into one HTTP request
await Promise.all([
  client.users.byId.query({ id: '1' }),
  client.users.byId.query({ id: '2' }),
  client.posts.list.query(),
]);
```

---

## React Query Integration

### Prefetching

```typescript
// SSR prefetching
export async function getServerSideProps() {
  const ssg = createProxySSGHelpers({
    router: appRouter,
    ctx: await createContext(),
    transformer: superjson,
  });

  await ssg.users.list.prefetch();

  return {
    props: {
      trpcState: ssg.dehydrate(),
    },
  };
}
```

### Optimistic Updates

```typescript
const utils = trpc.useContext();

const updateUser = trpc.users.update.useMutation({
  onMutate: async (newUser) => {
    // Cancel outgoing refetches
    await utils.users.byId.cancel({ id: newUser.id });

    // Snapshot previous value
    const previousUser = utils.users.byId.getData({ id: newUser.id });

    // Optimistically update
    utils.users.byId.setData({ id: newUser.id }, newUser);

    return { previousUser };
  },
  onError: (err, newUser, context) => {
    // Rollback on error
    utils.users.byId.setData({ id: newUser.id }, context?.previousUser);
  },
  onSettled: (data, error, variables) => {
    // Refetch
    utils.users.byId.invalidate({ id: variables.id });
  },
});
```

---

## Express Adapter

```typescript
// server.ts
import express from 'express';
import { createExpressMiddleware } from '@trpc/server/adapters/express';
import { appRouter } from './router';
import { createContext } from './context';

const app = express();

app.use(
  '/trpc',
  createExpressMiddleware({
    router: appRouter,
    createContext,
  })
);

app.listen(3000);
```

---

## OpenAPI Integration

```typescript
// Generate OpenAPI spec
import { generateOpenApiDocument } from 'trpc-openapi';

const openApiDocument = generateOpenApiDocument(appRouter, {
  title: 'My API',
  version: '1.0.0',
  baseUrl: 'http://localhost:3000',
});

// Add metadata to procedures
createUser: publicProcedure
  .meta({ openapi: { method: 'POST', path: '/users' } })
  .input(userSchema)
  .output(userSchema)
  .mutation(({ input }) => createUser(input)),
```

---

## Testing

### Unit Tests

```typescript
import { appRouter } from './router';
import { createContext } from './context';

describe('users router', () => {
  it('should create user', async () => {
    const ctx = await createContext({} as any);
    const caller = appRouter.createCaller(ctx);

    const user = await caller.users.create({
      email: 'test@example.com',
      name: 'Test User',
    });

    expect(user).toMatchObject({
      email: 'test@example.com',
      name: 'Test User',
    });
  });
});
```

### E2E Tests

```typescript
import { createTRPCMsw } from 'msw-trpc';
import { AppRouter } from './router';

const trpcMsw = createTRPCMsw<AppRouter>();

const handlers = [
  trpcMsw.users.list.query(() => {
    return [{ id: '1', name: 'Test' }];
  }),
];
```

---

## Best Practices

### Type Safety

```typescript
// ✅ DO: Export types
export type AppRouter = typeof appRouter;

// ✅ DO: Use inference
type RouterInput = inferRouterInputs<AppRouter>;
type RouterOutput = inferRouterOutputs<AppRouter>;
```

### Error Handling

```typescript
// ✅ DO: Use appropriate error codes
throw new TRPCError({
  code: 'NOT_FOUND',
  message: 'User not found',
});

// Codes: BAD_REQUEST, UNAUTHORIZED, FORBIDDEN, NOT_FOUND,
//        CONFLICT, INTERNAL_SERVER_ERROR, etc.
```

### Input Validation

```typescript
// ✅ DO: Always validate inputs
.input(z.object({
  email: z.string().email(),
  age: z.number().positive(),
}))

// ❌ DON'T: Skip validation
.input(z.any()) // Bad!
```

---

## Resources

### Documentation

- [tRPC](https://trpc.io/)
- [Zod](https://zod.dev/)
- [TanStack Query](https://tanstack.com/query/)
- [Next.js with tRPC](https://trpc.io/docs/nextjs)

### Examples

- [tRPC Examples](https://github.com/trpc/examples-next-prisma-starter)
- [Create T3 App](https://create.t3.gg/)

---

## Support

For issues or questions:
1. Check the tRPC documentation
2. Review examples repository
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
