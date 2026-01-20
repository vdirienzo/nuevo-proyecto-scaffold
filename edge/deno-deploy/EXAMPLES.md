# {{PROJECT_NAME}} - Code Examples

Practical examples for common use cases.

## Table of Contents

- [Authentication Flow](#authentication-flow)
- [CRUD Operations](#crud-operations)
- [Real-time Features](#real-time-features)
- [Caching Strategies](#caching-strategies)
- [Database Operations](#database-operations)
- [Background Jobs](#background-jobs)
- [Custom Middleware](#custom-middleware)

## Authentication Flow

### Complete User Registration and Login

```typescript
// Register new user
const registerUser = async (email: string, password: string, name?: string) => {
  const response = await fetch("http://localhost:8000/auth/register", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password, name }),
  });

  const data = await response.json();

  if (data.success) {
    // Store token
    localStorage.setItem("token", data.data.token);
    return data.data;
  } else {
    throw new Error(data.error);
  }
};

// Login existing user
const loginUser = async (email: string, password: string) => {
  const response = await fetch("http://localhost:8000/auth/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password }),
  });

  const data = await response.json();

  if (data.success) {
    localStorage.setItem("token", data.data.token);
    return data.data;
  } else {
    throw new Error(data.error);
  }
};

// Make authenticated request
const fetchProfile = async () => {
  const token = localStorage.getItem("token");

  const response = await fetch("http://localhost:8000/auth/me", {
    headers: {
      "Authorization": `Bearer ${token}`,
    },
  });

  return await response.json();
};
```

### Token Refresh Strategy

```typescript
// Refresh token before expiry
const refreshToken = async () => {
  const token = localStorage.getItem("token");

  const response = await fetch("http://localhost:8000/auth/refresh", {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${token}`,
    },
  });

  const data = await response.json();

  if (data.success) {
    localStorage.setItem("token", data.data.token);
    return data.data.token;
  }
};

// Auto-refresh setup
setInterval(async () => {
  try {
    await refreshToken();
    console.log("Token refreshed");
  } catch (error) {
    console.error("Failed to refresh token:", error);
    // Redirect to login
    window.location.href = "/login";
  }
}, 6 * 60 * 60 * 1000); // Every 6 hours
```

## CRUD Operations

### Complete Item Management

```typescript
// List all items
const listItems = async () => {
  const response = await fetch("http://localhost:8000/api/items");
  const data = await response.json();
  return data.data;
};

// Get single item
const getItem = async (id: string) => {
  const response = await fetch(`http://localhost:8000/api/items/${id}`);
  const data = await response.json();
  return data.data;
};

// Create item
const createItem = async (item: { name: string; description?: string; status?: string }) => {
  const token = localStorage.getItem("token");

  const response = await fetch("http://localhost:8000/api/items", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${token}`,
    },
    body: JSON.stringify(item),
  });

  return await response.json();
};

// Update item
const updateItem = async (id: string, updates: Partial<Item>) => {
  const token = localStorage.getItem("token");

  const response = await fetch(`http://localhost:8000/api/items/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${token}`,
    },
    body: JSON.stringify(updates),
  });

  return await response.json();
};

// Delete item
const deleteItem = async (id: string) => {
  const token = localStorage.getItem("token");

  await fetch(`http://localhost:8000/api/items/${id}`, {
    method: "DELETE",
    headers: {
      "Authorization": `Bearer ${token}`,
    },
  });
};
```

## Real-time Features

### WebSocket Chat Application

```typescript
// Client-side WebSocket connection
class ChatClient {
  private ws: WebSocket | null = null;
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 5;

  connect() {
    this.ws = new WebSocket("ws://localhost:8000/ws");

    this.ws.onopen = () => {
      console.log("Connected to chat");
      this.reconnectAttempts = 0;
    };

    this.ws.onmessage = (event) => {
      const message = JSON.parse(event.data);
      this.handleMessage(message);
    };

    this.ws.onclose = () => {
      console.log("Disconnected from chat");
      this.reconnect();
    };

    this.ws.onerror = (error) => {
      console.error("WebSocket error:", error);
    };
  }

  send(content: string) {
    if (this.ws?.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify({ content }));
    }
  }

  handleMessage(message: any) {
    switch (message.type) {
      case "join":
        console.log(`User ${message.userId} joined`);
        break;
      case "message":
        console.log(`${message.userId}: ${message.content}`);
        break;
      case "leave":
        console.log(`User ${message.userId} left`);
        break;
    }
  }

  reconnect() {
    if (this.reconnectAttempts < this.maxReconnectAttempts) {
      this.reconnectAttempts++;
      const delay = Math.min(1000 * Math.pow(2, this.reconnectAttempts), 30000);
      setTimeout(() => this.connect(), delay);
    }
  }

  disconnect() {
    this.ws?.close();
  }
}

// Usage
const chat = new ChatClient();
chat.connect();
chat.send("Hello, world!");
```

## Caching Strategies

### Multi-level Caching

```typescript
import { cacheService } from "./src/services/cache.ts";
import { kvService } from "./src/services/kv.ts";

// Simple cache-aside
const getUserCached = async (userId: string) => {
  return await cacheService.getOrSet(
    `user:${userId}`,
    async () => {
      // Fetch from database
      return await query("SELECT * FROM users WHERE id = $1", [userId]);
    },
    3600 * 1000, // 1 hour TTL
  );
};

// Cache invalidation
const updateUser = async (userId: string, updates: Partial<User>) => {
  // Update database
  await execute("UPDATE users SET name = $1 WHERE id = $2", [updates.name, userId]);

  // Invalidate cache
  await cacheService.delete(`user:${userId}`);

  // Return updated user
  return await getUserCached(userId);
};

// Batch cache warming
const warmCache = async (userIds: string[]) => {
  const promises = userIds.map(async (id) => {
    const user = await query("SELECT * FROM users WHERE id = $1", [id]);
    await cacheService.set(`user:${id}`, user, 3600 * 1000);
  });

  await Promise.all(promises);
};
```

### Cache with Stale-While-Revalidate

```typescript
const getCachedWithRevalidate = async <T>(
  key: string,
  factory: () => Promise<T>,
  ttl: number = 3600 * 1000,
): Promise<T> => {
  const cached = await cacheService.get<{ data: T; timestamp: number }>(key);

  if (cached) {
    const age = Date.now() - cached.timestamp;

    // If fresh, return immediately
    if (age < ttl / 2) {
      return cached.data;
    }

    // If stale but not expired, return and revalidate in background
    if (age < ttl) {
      // Background revalidation
      factory().then((fresh) => {
        cacheService.set(key, { data: fresh, timestamp: Date.now() }, ttl);
      });
      return cached.data;
    }
  }

  // No cache or expired, fetch fresh data
  const fresh = await factory();
  await cacheService.set(key, { data: fresh, timestamp: Date.now() }, ttl);
  return fresh;
};
```

## Database Operations

### Transaction Management

```typescript
import { getPool } from "./src/services/database.ts";

const transferItems = async (fromUserId: string, toUserId: string, itemId: string) => {
  const pool = getPool();
  const client = await pool.connect();

  try {
    // Start transaction
    await client.queryObject("BEGIN");

    // Check ownership
    const item = await client.queryObject<{ user_id: string }>(
      "SELECT user_id FROM items WHERE id = $1 FOR UPDATE",
      [itemId],
    );

    if (item.rows[0]?.user_id !== fromUserId) {
      throw new Error("Item not owned by sender");
    }

    // Transfer ownership
    await client.queryObject(
      "UPDATE items SET user_id = $1, updated_at = NOW() WHERE id = $2",
      [toUserId, itemId],
    );

    // Log transfer
    await client.queryObject(
      "INSERT INTO transfer_logs (item_id, from_user_id, to_user_id) VALUES ($1, $2, $3)",
      [itemId, fromUserId, toUserId],
    );

    // Commit transaction
    await client.queryObject("COMMIT");

    return { success: true };
  } catch (error) {
    // Rollback on error
    await client.queryObject("ROLLBACK");
    throw error;
  } finally {
    client.release();
  }
};
```

### Pagination with Cursor

```typescript
interface PaginatedResult<T> {
  items: T[];
  nextCursor: string | null;
  hasMore: boolean;
}

const paginateItems = async <T>(
  cursor: string | null,
  limit: number = 20,
): Promise<PaginatedResult<T>> => {
  const query = cursor
    ? "SELECT * FROM items WHERE id > $1 ORDER BY id LIMIT $2"
    : "SELECT * FROM items ORDER BY id LIMIT $1";

  const params = cursor ? [cursor, limit + 1] : [limit + 1];

  const results = await query<T & { id: string }>(query, params);

  const hasMore = results.length > limit;
  const items = hasMore ? results.slice(0, limit) : results;
  const nextCursor = hasMore ? items[items.length - 1].id : null;

  return {
    items,
    nextCursor,
    hasMore,
  };
};
```

## Background Jobs

### Scheduled Tasks with Deno.cron

```typescript
// Data cleanup job
Deno.cron("cleanup-expired-sessions", "0 * * * *", async () => {
  console.log("Cleaning up expired sessions...");

  const expiredSessions = await kvService.listByPrefix("session:");

  for (const session of expiredSessions) {
    // Check if expired
    const sessionData = await kvService.get(session as string);
    if (sessionData && isExpired(sessionData)) {
      await kvService.delete(session as string);
    }
  }

  console.log("Cleanup completed");
});

// Daily report generation
Deno.cron("generate-daily-report", "0 0 * * *", async () => {
  console.log("Generating daily report...");

  const stats = await query(`
    SELECT
      COUNT(*) as total_users,
      COUNT(CASE WHEN created_at >= NOW() - INTERVAL '24 hours' THEN 1 END) as new_users
    FROM users
  `);

  // Send report via email or store
  await kvService.set("daily-report", stats, 7 * 24 * 60 * 60 * 1000);

  console.log("Report generated");
});

// Health monitoring
Deno.cron("health-monitor", "*/5 * * * *", async () => {
  try {
    // Check database
    await query("SELECT 1");

    // Check external services
    const response = await fetch("https://api.example.com/health");

    if (!response.ok) {
      console.error("External service unhealthy");
      // Send alert
    }
  } catch (error) {
    console.error("Health check failed:", error);
    // Send alert
  }
});
```

## Custom Middleware

### Rate Limiting Middleware

```typescript
import type { Context, Next } from "oak";
import { kvService } from "../services/kv.ts";

interface RateLimitConfig {
  windowMs: number;
  maxRequests: number;
}

export function rateLimitMiddleware(config: RateLimitConfig) {
  return async (ctx: Context, next: Next) => {
    const ip = ctx.request.ip;
    const key = `rate-limit:${ip}`;

    const current = await kvService.get<{ count: number; resetAt: number }>(key);
    const now = Date.now();

    if (current && current.resetAt > now) {
      if (current.count >= config.maxRequests) {
        ctx.response.status = 429;
        ctx.response.body = {
          error: "Too many requests",
          retryAfter: Math.ceil((current.resetAt - now) / 1000),
        };
        return;
      }

      await kvService.set(
        key,
        { count: current.count + 1, resetAt: current.resetAt },
        current.resetAt - now,
      );
    } else {
      await kvService.set(
        key,
        { count: 1, resetAt: now + config.windowMs },
        config.windowMs,
      );
    }

    await next();
  };
}

// Usage
app.use(rateLimitMiddleware({ windowMs: 60000, maxRequests: 100 }));
```

### Request ID Middleware

```typescript
import type { Context, Next } from "oak";

export async function requestIdMiddleware(ctx: Context, next: Next) {
  const requestId = ctx.request.headers.get("X-Request-ID") || crypto.randomUUID();

  ctx.state.requestId = requestId;
  ctx.response.headers.set("X-Request-ID", requestId);

  const start = Date.now();

  await next();

  const duration = Date.now() - start;
  console.log(`[${requestId}] ${ctx.request.method} ${ctx.request.url.pathname} - ${duration}ms`);
}
```

## Testing Examples

### Integration Test with Database

```typescript
import { assertEquals } from "@std/assert";
import { Pool } from "postgres";

Deno.test("User CRUD operations", async () => {
  const pool = new Pool("postgresql://test:test@localhost:5432/test", 1, true);

  try {
    // Setup
    const client = await pool.connect();
    await client.queryObject("DELETE FROM users");

    // Create user
    const insertResult = await client.queryObject<{ id: string }>(
      "INSERT INTO users (email, name, password_hash) VALUES ($1, $2, $3) RETURNING id",
      ["test@example.com", "Test User", "hash"],
    );

    const userId = insertResult.rows[0].id;
    assertEquals(typeof userId, "string");

    // Read user
    const selectResult = await client.queryObject<{ email: string }>(
      "SELECT email FROM users WHERE id = $1",
      [userId],
    );

    assertEquals(selectResult.rows[0].email, "test@example.com");

    // Update user
    await client.queryObject(
      "UPDATE users SET name = $1 WHERE id = $2",
      ["Updated Name", userId],
    );

    // Delete user
    await client.queryObject("DELETE FROM users WHERE id = $1", [userId]);

    client.release();
  } finally {
    await pool.end();
  }
});
```

## Author

{{AUTHOR}}
