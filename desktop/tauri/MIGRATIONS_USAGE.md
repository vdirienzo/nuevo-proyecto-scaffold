# Using Database Migrations in Tauri Projects

Author: Homero Thompson del Lago del Terror

## Quick Start

### 1. Add Dependencies to Cargo.toml

```toml
[dependencies]
sqlx = { version = "0.7", features = ["runtime-tokio-rustls", "sqlite", "migrate"] }
tokio = { version = "1", features = ["full"] }
dirs = "5.0"
log = "0.4"

[build-dependencies]
sqlx = { version = "0.7", features = ["sqlite", "migrate"] }
```

### 2. Update Your main.rs

```rust
use sqlx::SqlitePool;
use tauri::Manager;

mod migrations;
mod seeds;

#[tokio::main]
async fn main() {
    env_logger::init();

    tauri::Builder::default()
        .setup(|app| {
            let app_name = app.package_info().name.clone();

            tauri::async_runtime::block_on(async move {
                // Initialize database with migrations
                let db_url = migrations::init_database(&app_name)
                    .await
                    .expect("Failed to initialize database");

                // Load seed data (dev only)
                if cfg!(debug_assertions) {
                    seeds::init_seeds(&db_url).await.ok();
                }

                // Create connection pool
                let pool = SqlitePool::connect(&db_url)
                    .await
                    .expect("Failed to connect to database");

                // Store pool in app state
                app.manage(pool);
            });

            Ok(())
        })
        .invoke_handler(tauri::generate_handler![
            // Your commands here
        ])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

### 3. Update Your lib.rs

```rust
pub mod migrations;
pub mod seeds;
pub mod db;
pub mod models;
pub mod services;
```

### 4. Set Up .env for Development

```env
APP_ENV=development
RUST_LOG=info
```

### 5. Run Your App

```bash
# Development mode (with seeds)
cargo tauri dev

# Production build (no seeds)
cargo tauri build
```

## Database Location

The database is stored in:
- **Linux**: `~/.local/share/{app-name}/database.db`
- **macOS**: `~/Library/Application Support/{app-name}/database.db`
- **Windows**: `C:\Users\{user}\AppData\Local\{app-name}\database.db`

## Using Database in Commands

```rust
use sqlx::SqlitePool;
use tauri::State;

#[tauri::command]
async fn get_users(pool: State<'_, SqlitePool>) -> Result<Vec<User>, String> {
    let users = sqlx::query_as::<_, User>("SELECT * FROM users")
        .fetch_all(pool.inner())
        .await
        .map_err(|e| e.to_string())?;

    Ok(users)
}
```

## Development Commands

### View Database

```bash
# Install sqlite3
sudo apt-get install sqlite3  # Linux
brew install sqlite3          # macOS

# Open database
sqlite3 ~/.local/share/your-app-name/database.db

# Run queries
.tables
SELECT * FROM users;
.schema users
.exit
```

### Reset Database (Dev Only)

Add this command to your app:

```rust
#[cfg(debug_assertions)]
#[tauri::command]
async fn dev_reset_database(
    pool: State<'_, SqlitePool>,
    app_handle: tauri::AppHandle
) -> Result<String, String> {
    let app_name = app_handle.package_info().name.clone();

    // Close current pool
    pool.inner().close().await;

    // Delete database file
    let db_path = crate::migrations::get_database_path(&app_name)
        .map_err(|e| e.to_string())?;
    std::fs::remove_file(&db_path).ok();

    // Reinitialize
    let db_url = crate::migrations::init_database(&app_name)
        .await
        .map_err(|e| e.to_string())?;

    crate::seeds::init_seeds(&db_url)
        .await
        .map_err(|e| e.to_string())?;

    Ok("Database reset successfully".to_string())
}
```

## Troubleshooting

### Migration Errors

```bash
# Check sqlx CLI is installed
cargo install sqlx-cli --no-default-features --features sqlite

# Verify migrations
sqlx migrate info --database-url "sqlite://./database.db"

# Check migration status
sqlx migrate run --database-url "sqlite://./database.db"
```

### Compile-Time Verification

If you get sqlx compile errors:

```bash
# Generate sqlx-data.json
cargo sqlx prepare --database-url "sqlite://./database.db"

# Or disable compile-time checks (not recommended)
# In Cargo.toml:
[dependencies]
sqlx = { version = "0.7", features = ["runtime-tokio-rustls", "sqlite"], default-features = false }
```

### Seed Data Not Loading

Check logs:
```bash
RUST_LOG=debug cargo tauri dev
```

Should see: "Running development seed data..."

If not, check:
- `APP_ENV=development` is set
- Running in debug mode
- No errors in seed SQL syntax

## Best Practices

1. **Never modify existing migrations** - Always create new ones
2. **Test migrations** - Use `cargo test` before committing
3. **Backup data** - Before running new migrations in production
4. **Use transactions** - For complex data migrations
5. **Document changes** - Update migration READMEs
6. **Version control** - Commit all migration files

## Advanced: Custom Migrations

Create a new migration:

```bash
# Create file: src-tauri/migrations/00003_add_feature.sql.tmpl
```

```sql
-- Add new feature
CREATE TABLE IF NOT EXISTS feature_table (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    created_at INTEGER NOT NULL DEFAULT (strftime('%s', 'now'))
);

CREATE INDEX idx_feature_name ON feature_table(name);

-- Migrate existing data if needed
INSERT INTO feature_table (name)
SELECT DISTINCT some_field FROM other_table;
```

## Testing Migrations

```rust
#[cfg(test)]
mod tests {
    use sqlx::SqlitePool;

    #[tokio::test]
    async fn test_migration_idempotent() {
        let pool = SqlitePool::connect("sqlite::memory:").await.unwrap();

        // Run migrations twice
        crate::migrations::run_migrations("sqlite::memory:").await.unwrap();
        crate::migrations::run_migrations("sqlite::memory:").await.unwrap();

        // Should not error
        pool.close().await;
    }
}
```

## Further Reading

- [SQLx Documentation](https://docs.rs/sqlx)
- [Tauri State Management](https://tauri.app/v1/guides/features/state-management)
- [SQLite Best Practices](https://www.sqlite.org/bestpractice.html)
