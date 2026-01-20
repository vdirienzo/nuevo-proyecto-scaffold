# Database Seeds

Author: Homero Thompson del Lago del Terror

## Overview

Development seed data for local testing. Seeds are **only** loaded in development mode and provide sample users, settings, and audit logs.

## Seed Files

| File | Description |
|------|-------------|
| `dev_seed.sql` | Development seed data (users, settings, sessions) |
| `seed_runner.rs` | Rust seed runner with dev mode detection |

## Seed Data Included

### Users
| Email | Password | Role | Status |
|-------|----------|------|--------|
| {{DEFAULT_ADMIN_EMAIL}} | `admin123` | Admin | Active |
| test@example.com | `test123` | User | Active |

**⚠️ SECURITY WARNING:** Change default passwords immediately if deploying!

### Global Settings
- `app.theme`: "light"
- `app.language`: "en"
- `app.telemetry_enabled`: false
- `app.auto_update`: true
- `app.version`: "1.0.0"

### User Settings
- UI preferences (sidebar, notifications, theme)
- Sample configuration for both users

### Sessions
- Long-lived development session token for admin (30 days)
- Useful for testing without repeated logins

## How Seeds Work

1. **Dev mode detection**:
   - Checks `APP_ENV` environment variable
   - Falls back to `debug_assertions` if not set

2. **Automatic loading**:
   - Called after migrations in `main.rs`
   - Skipped automatically in production builds

3. **Idempotent**:
   - Uses `INSERT OR IGNORE`
   - Safe to run multiple times

## Environment Detection

```rust
// Development
env::set_var("APP_ENV", "development");

// Production
env::set_var("APP_ENV", "production");
```

## Usage in main.rs

```rust
use crate::migrations::init_database;
use crate::seeds::init_seeds;

#[tokio::main]
async fn main() {
    // Initialize database with migrations
    let db_url = init_database("{{PROJECT_NAME}}").await?;

    // Load seed data (dev only)
    init_seeds(&db_url).await?;
}
```

## Development Reset (Debug Only)

```rust
#[cfg(debug_assertions)]
use crate::seeds::reset_database;

// Completely wipe database - fresh start
reset_database(&pool).await?;
```

## Testing Seeds

```bash
# Set development mode
export APP_ENV=development

# Run app
cargo tauri dev

# Check logs
# Should see: "Running development seed data..."
```

## Production Safety

Seeds will **never** run in production if:
- `APP_ENV != "development"`
- Built with `--release` (no debug_assertions)
- Environment variable explicitly set to production

## Adding New Seed Data

1. Edit `dev_seed.sql.tmpl`
2. Use `INSERT OR IGNORE` for idempotency
3. Add appropriate comments
4. Test with `cargo test`

Example:
```sql
INSERT OR IGNORE INTO users (email, password_hash, is_active, is_admin)
VALUES ('newuser@example.com', '$argon2id$...', 1, 0);
```

## Password Hashing

Seed passwords are hashed with **argon2id** (v19):
- Memory: 19456 KB
- Iterations: 2
- Parallelism: 1

To generate new hashes:
```rust
use argon2::{Argon2, PasswordHasher};
use argon2::password_hash::{SaltString, rand_core::OsRng};

let salt = SaltString::generate(&mut OsRng);
let argon2 = Argon2::default();
let hash = argon2.hash_password(b"password", &salt)?;
println!("{}", hash);
```

## Best Practices

- ✅ Always use `INSERT OR IGNORE`
- ✅ Include comments explaining seed purpose
- ✅ Use realistic test data
- ✅ Keep passwords secure (even in dev)
- ❌ Never commit real user data
- ❌ Don't hardcode production credentials
- ❌ Avoid excessive seed data (keep it minimal)
