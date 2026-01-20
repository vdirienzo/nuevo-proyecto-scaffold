# Database Schema Documentation

Author: Homero Thompson del Lago del Terror
Project: {{PROJECT_NAME}}

## Schema Overview

This document describes the complete SQLite database schema for the Tauri desktop application.

## Entity Relationship Diagram

```
┌──────────────────┐
│      users       │
├──────────────────┤
│ id (PK)          │◄────┐
│ email            │     │
│ password_hash    │     │
│ is_active        │     │
│ is_admin         │     │
│ created_at       │     │
│ updated_at       │     │
└──────────────────┘     │
         │               │
         │               │
         ▼               │
┌──────────────────┐     │
│    sessions      │     │
├──────────────────┤     │
│ id (PK)          │     │
│ user_id (FK)     │─────┘
│ token            │
│ expires_at       │
│ ip_address       │
│ user_agent       │
│ created_at       │
└──────────────────┘
         │
         │
         │
┌──────────────────┐     ┌──────────────────┐
│    settings      │     │   audit_logs     │
├──────────────────┤     ├──────────────────┤
│ id (PK)          │     │ id (PK)          │
│ key              │     │ action           │
│ value            │     │ entity_type      │
│ user_id (FK)     │────►│ entity_id        │
│ is_global        │     │ user_id (FK)     │
│ created_at       │     │ payload          │
│ updated_at       │     │ ip_address       │
└──────────────────┘     │ user_agent       │
                         │ severity         │
                         │ created_at       │
                         └──────────────────┘
```

## Tables

### users

Stores user authentication and profile information.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | Unique user identifier |
| email | TEXT | NOT NULL UNIQUE | User email address |
| password_hash | TEXT | NOT NULL | Argon2id hashed password |
| is_active | INTEGER | NOT NULL DEFAULT 1 | Account status (1=active, 0=disabled) |
| is_admin | INTEGER | NOT NULL DEFAULT 0 | Admin privileges (1=admin, 0=regular) |
| created_at | INTEGER | NOT NULL | Unix timestamp of creation |
| updated_at | INTEGER | NOT NULL | Unix timestamp of last update |

**Indexes:**
- `idx_users_email` on `email`
- `idx_users_active` on `is_active`
- `idx_users_created_at` on `created_at`

**Triggers:**
- `update_users_timestamp` - Auto-updates `updated_at` on UPDATE

---

### settings

Stores application and user-specific configuration settings.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | Unique setting identifier |
| key | TEXT | NOT NULL | Setting key (e.g., "app.theme") |
| value | TEXT | NULL | Setting value (JSON compatible) |
| user_id | INTEGER | FOREIGN KEY (users.id) | User owner (NULL for global) |
| is_global | INTEGER | NOT NULL DEFAULT 0 | Global vs user-specific |
| created_at | INTEGER | NOT NULL | Unix timestamp of creation |
| updated_at | INTEGER | NOT NULL | Unix timestamp of last update |

**Constraints:**
- `UNIQUE(key, user_id)` - One setting per user per key
- `CHECK` - Settings must be either user-specific OR global, not both

**Indexes:**
- `idx_settings_key` on `key`
- `idx_settings_user_id` on `user_id`
- `idx_settings_global` on `is_global`

**Triggers:**
- `update_settings_timestamp` - Auto-updates `updated_at` on UPDATE

---

### sessions

Stores active user sessions and authentication tokens.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | Unique session identifier |
| user_id | INTEGER | NOT NULL FOREIGN KEY (users.id) | Session owner |
| token | TEXT | NOT NULL UNIQUE | Session token |
| expires_at | INTEGER | NOT NULL | Unix timestamp of expiration |
| ip_address | TEXT | NULL | Client IP address |
| user_agent | TEXT | NULL | Client user agent |
| created_at | INTEGER | NOT NULL | Unix timestamp of creation |

**Indexes:**
- `idx_sessions_token` on `token`
- `idx_sessions_user_id` on `user_id`
- `idx_sessions_expires_at` on `expires_at`
- `idx_sessions_created_at` on `created_at`

**Triggers:**
- `clean_expired_sessions` - Auto-deletes expired sessions on INSERT

---

### audit_logs

Comprehensive audit trail of all system events.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | Unique log identifier |
| action | TEXT | NOT NULL | Action performed (e.g., "user.created") |
| entity_type | TEXT | NOT NULL | Type of entity affected |
| entity_id | INTEGER | NULL | ID of entity affected |
| user_id | INTEGER | FOREIGN KEY (users.id) | User who performed action |
| payload | TEXT | NULL | JSON payload with details |
| ip_address | TEXT | NULL | Client IP address |
| user_agent | TEXT | NULL | Client user agent |
| severity | TEXT | NOT NULL DEFAULT 'info' | Log severity level |
| created_at | INTEGER | NOT NULL | Unix timestamp of event |

**Indexes:**
- `idx_audit_logs_action` on `action`
- `idx_audit_logs_entity` on `(entity_type, entity_id)`
- `idx_audit_logs_user_id` on `user_id`
- `idx_audit_logs_created_at` on `created_at`
- `idx_audit_logs_severity` on `severity`

**Triggers:**
- `log_user_creation` - Auto-logs user creation
- `log_user_update` - Auto-logs user updates
- `log_user_deletion` - Auto-logs user deletion
- `log_session_creation` - Auto-logs session creation

---

## Views

### recent_audit_logs

Convenience view for recent audit events with user email join.

```sql
SELECT
    al.id,
    al.action,
    al.entity_type,
    al.entity_id,
    u.email as user_email,
    al.payload,
    al.severity,
    datetime(al.created_at, 'unixepoch') as created_at_readable,
    al.created_at
FROM audit_logs al
LEFT JOIN users u ON al.user_id = u.id
ORDER BY al.created_at DESC
LIMIT 1000
```

---

## Common Queries

### Get Active Users

```sql
SELECT * FROM users WHERE is_active = 1;
```

### Get User Settings

```sql
SELECT key, value
FROM settings
WHERE user_id = ? AND is_global = 0;
```

### Get Global Settings

```sql
SELECT key, value
FROM settings
WHERE is_global = 1;
```

### Validate Session

```sql
SELECT s.*, u.*
FROM sessions s
JOIN users u ON s.user_id = u.id
WHERE s.token = ?
  AND s.expires_at > strftime('%s', 'now')
  AND u.is_active = 1;
```

### Recent User Actions

```sql
SELECT * FROM recent_audit_logs
WHERE user_email = 'user@example.com'
LIMIT 50;
```

### Clean Expired Sessions

```sql
DELETE FROM sessions
WHERE expires_at < strftime('%s', 'now');
```

---

## Data Types

SQLite uses dynamic typing with the following storage classes:

| Column Type | Storage Class | Description |
|-------------|---------------|-------------|
| INTEGER | INTEGER | 1-8 bytes signed integer |
| TEXT | TEXT | UTF-8 encoded string |
| NULL | NULL | Missing value |

**Note:** All timestamps are stored as INTEGER Unix timestamps (seconds since epoch).

---

## Security Considerations

### Password Storage

- ✅ Uses **Argon2id** (memory-hard, GPU-resistant)
- ✅ Parameters: m=19456, t=2, p=1
- ❌ Never store plaintext passwords
- ❌ Never log password hashes

### Session Management

- ✅ Tokens stored with expiration
- ✅ Auto-cleanup of expired sessions
- ✅ IP and user agent tracking
- ⚠️ Consider rotating tokens on sensitive operations

### Audit Logging

- ✅ All user CRUD operations logged automatically
- ✅ Severity levels for filtering
- ✅ Immutable logs (no UPDATE/DELETE triggers on audit_logs)
- ⚠️ Consider log rotation for production

---

## Performance Considerations

### Indexes

All frequently queried columns have indexes:
- Email lookups: `idx_users_email`
- Session validation: `idx_sessions_token`, `idx_sessions_expires_at`
- Audit queries: `idx_audit_logs_created_at`, `idx_audit_logs_action`

### Optimization Tips

1. **Use prepared statements** - Prevent SQL injection, improve performance
2. **Batch operations** - Use transactions for multiple INSERTs
3. **Limit result sets** - Always use LIMIT for unbounded queries
4. **Archive old data** - Move old audit_logs to archive table

### Query Performance

```sql
-- Good: Uses index
SELECT * FROM users WHERE email = 'user@example.com';

-- Bad: Full table scan
SELECT * FROM users WHERE LOWER(email) = 'user@example.com';

-- Good: Limited results
SELECT * FROM audit_logs ORDER BY created_at DESC LIMIT 100;

-- Bad: Unbounded
SELECT * FROM audit_logs ORDER BY created_at DESC;
```

---

## Migration History

| Version | File | Description |
|---------|------|-------------|
| 00001 | `00001_initial_schema.sql` | Initial schema with users, settings, sessions |
| 00002 | `00002_add_audit_log.sql` | Audit logging with automatic triggers |

---

## Development Seed Data

When running in development mode (`APP_ENV=development`), the following seed data is loaded:

**Users:**
- Admin: `{{DEFAULT_ADMIN_EMAIL}}` / `admin123`
- Test: `test@example.com` / `test123`

**Global Settings:**
- `app.theme`: "light"
- `app.language`: "en"
- `app.telemetry_enabled`: false
- `app.auto_update`: true

⚠️ **Change default passwords before deploying!**

---

## Further Reading

- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [SQLx Migrations](https://docs.rs/sqlx/latest/sqlx/migrate/index.html)
- [Argon2 Password Hashing](https://en.wikipedia.org/wiki/Argon2)
- [OWASP Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
