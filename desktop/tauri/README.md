# Tauri Desktop Application Templates

> Cross-platform desktop applications with Rust backend and web frontend

**Author:** Homero Thompson del Lago del Terror
**Version:** 6.1.0

---

## Overview

Tauri is a framework for building lightweight, secure desktop applications using:
- **Backend:** Rust (memory-safe, high performance)
- **Frontend:** Any web framework (React, Vue, vanilla JS)
- **Bundle size:** ~3-10MB (vs Electron's 150MB+)

---

## Template Structure

```
desktop/tauri/
├── src-tauri/                    # Rust backend
│   ├── Cargo.toml.tmpl          # Dependencies
│   ├── tauri.conf.json.tmpl     # Tauri configuration
│   ├── build.rs.tmpl            # Build script
│   ├── capabilities/            # Security capabilities
│   │   └── default.json.tmpl
│   └── src/
│       ├── main.rs.tmpl         # Entry point
│       ├── lib.rs.tmpl          # Library root
│       ├── config.rs.tmpl       # App configuration
│       ├── error.rs.tmpl        # Error types
│       ├── commands/            # IPC commands
│       │   ├── mod.rs.tmpl
│       │   ├── app.rs.tmpl
│       │   ├── system.rs.tmpl
│       │   ├── auth.rs.tmpl
│       │   └── files.rs.tmpl
│       ├── state/               # Application state
│       │   ├── mod.rs.tmpl
│       │   └── app_state.rs.tmpl
│       ├── db/                  # Database layer
│       │   ├── mod.rs.tmpl
│       │   └── sqlite.rs.tmpl
│       ├── models/              # Data models
│       │   ├── mod.rs.tmpl
│       │   ├── user.rs.tmpl
│       │   └── settings.rs.tmpl
│       └── services/            # Business logic
│           ├── mod.rs.tmpl
│           ├── auth_service.rs.tmpl
│           └── settings_service.rs.tmpl
├── src/                         # Frontend (React/Vue)
│   └── lib/
│       └── tauri.ts.tmpl        # Tauri API wrapper
├── tests/                       # Test templates
│   ├── rust/
│   │   └── integration_test.rs.tmpl
│   └── e2e/
│       └── app.spec.ts.tmpl
├── .env.example.tmpl
├── Dockerfile.tmpl
├── docker-compose.yml.tmpl
└── .github/
    └── workflows/
        └── tauri-release.yml.tmpl
```

---

## Features

### Rust Backend
- **Type-safe IPC:** Commands with full type safety
- **SQLite database:** Embedded database with SQLx
- **Secure storage:** Encrypted credentials storage
- **File system access:** Controlled FS operations
- **System tray:** Native system tray support
- **Auto-updates:** Built-in update mechanism

### Frontend Integration
- **React SPA** (recommended) - Vite + TypeScript
- **Vue 3** - Composition API + Pinia
- **Vanilla JS** - Lightweight option

### Security
- **Capability-based permissions:** Fine-grained API access
- **CSP headers:** Content Security Policy
- **No Node.js runtime:** Smaller attack surface
- **Sandboxed webview:** Process isolation

---

## Usage

Generated projects include:

```bash
# Development
npm run tauri dev

# Build for current platform
npm run tauri build

# Build for all platforms (CI)
npm run tauri build -- --target all
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project identifier (kebab-case) | `my-desktop-app` |
| `{{AUTHOR}}` | Author name | `Homero Thompson del Lago del Terror` |
| `{{APP_TITLE}}` | Window title | `My Desktop App` |
| `{{BUNDLE_ID}}` | Bundle identifier | `com.example.mydesktopapp` |

---

## Capabilities (Tauri v2)

Templates use Tauri v2's capability system for security:

```json
{
  "identifier": "default",
  "windows": ["main"],
  "permissions": [
    "core:default",
    "shell:allow-open",
    "dialog:allow-open",
    "fs:allow-read"
  ]
}
```

---

## Cross-References

| Topic | Document |
|-------|----------|
| Frontend integration | `/frontend/react-spa/` or `/frontend/vue/` |
| Rust patterns | `/backend/rust/` |
| Testing | `/testing/e2e/` |
| CI/CD | `/ci-cd/` |
