# Database Migrations

Author: Homero Thompson del Lago del Terror

## Overview

This directory contains SQLite database migrations for the Tauri application. Migrations are run automatically on application startup using `sqlx::migrate!()`.

## Migration Files

| File | Description |
|------|-------------|
| `00001_initial_schema.sql` | Initial database schema with users, settings, sessions |
| `00002_add_audit_log.sql` | Audit logging system with automatic triggers |
| `mod.rs` | Migration runner module |

## Schema Overview

### Users Table
- Stores user authentication data
- Supports admin/regular user roles
- Tracks active/inactive status
- Automatic timestamp updates via triggers

### Settings Table
- User-specific and global application settings
- JSON-compatible values
- Enforces constraint: settings must be either user-specific OR global

### Sessions Table
- Active user sessions with tokens
- Automatic expiration tracking
- IP address and user agent logging
- Auto-cleanup of expired sessions via trigger

### Audit Logs Table
- Comprehensive event tracking
- Automatic logging of user CRUD operations
- Severity levels: info, warning, error
- JSON payload support

## How It Works

1. **Compile-time verification**: sqlx verifies migrations at compile time
2. **Automatic execution**: Migrations run on app startup via `init_database()`
3. **Idempotent**: Safe to run multiple times (uses `IF NOT EXISTS`)
4. **Ordered**: Executed in lexicographic order (00001, 00002, etc.)

## Adding New Migrations

1. Create a new file: `0000X_description.sql.tmpl`
2. Use SQLite syntax
3. Include `IF NOT EXISTS` clauses for safety
4. Test with in-memory database
5. Update this README

Example:
```sql
-- 00003_add_feature.sql.tmpl
CREATE TABLE IF NOT EXISTS new_table (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL
);
```

## Testing Migrations

```rust
#[tokio::test]
async fn test_migrations() {
    let db_url = "sqlite::memory:";
    run_migrations(db_url).await.expect("Migrations failed");
}
```

## SQLite Features Used

- `INTEGER PRIMARY KEY AUTOINCREMENT` - Auto-incrementing IDs
- `FOREIGN KEY ... ON DELETE CASCADE` - Referential integrity
- `UNIQUE` constraints
- `CHECK` constraints
- `TRIGGER` - Automatic timestamp updates and cleanup
- `INDEX` - Query optimization
- `VIEW` - Convenience queries

## Best Practices

- ✅ Always use `IF NOT EXISTS`
- ✅ Add indexes for frequently queried columns
- ✅ Use foreign keys with appropriate `ON DELETE` actions
- ✅ Include timestamps (created_at, updated_at)
- ✅ Use triggers for automatic behaviors
- ❌ Never modify existing migrations (create new ones instead)
- ❌ Don't use database-specific features (keep it SQLite-compatible)
