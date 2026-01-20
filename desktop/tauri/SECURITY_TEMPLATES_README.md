# Tauri Security Templates

**Author:** Homero Thompson del Lago del Terror
**Date:** 2026-01-20
**Version:** 1.0

## Overview

This directory contains comprehensive security templates for Tauri desktop applications, following OWASP guidelines and Tauri v2 best practices.

## Files Created

### Core Security Modules

| File | Purpose | Lines |
|------|---------|-------|
| `src-tauri/src/security.rs.tmpl` | CSRF, rate limiting, input sanitization, path validation | ~450 |
| `src-tauri/src/crypto.rs.tmpl` | Password hashing, encryption (AES-256-GCM), token generation | ~400 |
| `src-tauri/src/commands/security_example.rs.tmpl` | Example commands showing usage | ~300 |

### Capabilities (Permissions)

| File | Purpose | Security Level |
|------|---------|----------------|
| `src-tauri/capabilities/minimal.json.tmpl` | Sandboxed mode with essential permissions only | High |
| `src-tauri/capabilities/default.json.tmpl` | Balanced permissions for production | Medium |
| `src-tauri/capabilities/full.json.tmpl` | All permissions (development only) | Low ⚠️ |
| `src-tauri/capabilities/custom.json.tmpl` | Customizable template with comments | Custom |
| `src-tauri/capabilities/README.md` | Guide for using capabilities | - |

### Documentation

| File | Purpose |
|------|---------|
| `CSP.md.tmpl` | Content Security Policy guide |
| `SECURITY.md.tmpl` | Complete security implementation guide |
| `SECURITY_TEMPLATES_README.md` | This file |

### Configuration Updates

| File | Changes |
|------|---------|
| `src-tauri/Cargo.toml.tmpl` | Added: `aes-gcm`, `hex`, `base64`, `rand`, `regex` |
| `src-tauri/src/lib.rs.tmpl` | Added: `crypto`, `security` modules |

## Features

### 🔐 Password Security
- **Argon2id hashing** - Industry-standard password hashing
- **Automatic salt generation** - Unique salt per password
- **Constant-time comparison** - Prevents timing attacks
- **Rehashing detection** - Upgrade old hashes

### 🔒 Data Encryption
- **AES-256-GCM** - Military-grade encryption
- **Authenticated encryption** - Prevents tampering
- **Key derivation from passwords** - Using Argon2
- **Export/import keys** - For key management

### 🛡️ CSRF Protection
- **Token generation** - Secure random tokens
- **Token validation** - Constant-time comparison
- **Expiration handling** - 1-hour TTL by default
- **Automatic cleanup** - Remove expired tokens

### ⏱️ Rate Limiting
- **Configurable limits** - Requests per time window
- **Per-user/global** - Flexible rate limiting strategies
- **Brute-force prevention** - Protect login endpoints
- **Automatic cleanup** - Remove old request records

### 🧹 Input Sanitization
- **Control character removal** - Prevent injection attacks
- **Length limiting** - Prevent buffer overflows
- **Email validation** - RFC-compliant regex
- **Username validation** - Alphanumeric + `_` + `-`
- **HTML stripping** - Remove script tags
- **Alphanumeric checking** - For codes/tokens

### 📁 Path Validation
- **Directory traversal detection** - Block `../` patterns
- **Canonical path validation** - Ensure within allowed directory
- **Filename sanitization** - Remove dangerous characters
- **Whitelist approach** - Only allow specific directories

### 🎲 Token Generation
- **Cryptographically secure** - Using OS random source
- **API keys** - Prefixed with project name
- **Verification codes** - 6-digit numeric
- **Generic tokens** - URL-safe base64

### 🔑 Capabilities
- **Minimal** - Sandboxed with essential permissions
- **Default** - Balanced for production
- **Full** - Development only (⚠️ insecure)
- **Custom** - Commented template for customization

## Quick Start

### 1. Basic Usage

```rust
use crate::crypto::{PasswordHasher, Encryptor};
use crate::security::{CsrfManager, InputSanitizer};

// Hash password
let hash = PasswordHasher::hash_password("user_password")?;

// Encrypt data
let encryptor = Encryptor::new();
let encrypted = encryptor.encrypt_string("sensitive data")?;

// Sanitize input
let clean = InputSanitizer::sanitize_string(user_input, 200);

// Generate CSRF token
let csrf_manager = CsrfManager::new();
let token = csrf_manager.generate_token(session_id)?;
```

### 2. Setting Up State

In `src-tauri/src/main.rs`:

```rust
use crate::commands::security_example::SecurityState;

fn main() {
    tauri::Builder::default()
        .manage(Arc::new(SecurityState::new()))
        .invoke_handler(tauri::generate_handler![
            register_user,
            login,
            update_profile,
            // ... other commands
        ])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

### 3. Choosing a Capability

For production, use **minimal** or **custom**:

Edit `src-tauri/tauri.conf.json`:

```json
{
  "bundle": {
    "capabilities": ["minimal"]
  }
}
```

### 4. Configuring CSP

Edit `src-tauri/tauri.conf.json`:

```json
{
  "security": {
    "csp": "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; connect-src 'self' https://api.yourdomain.com"
  }
}
```

## Testing Security

### Run Tests

```bash
cd src-tauri
cargo test
```

### Check for Vulnerabilities

```bash
cargo install cargo-audit
cargo audit
```

### Lint Security Issues

```bash
cargo install cargo-clippy
cargo clippy -- -W clippy::all
```

## Integration Checklist

When integrating these templates into your project:

- [ ] Review `security.rs` and `crypto.rs` for project-specific needs
- [ ] Choose appropriate capability (`minimal` recommended for production)
- [ ] Configure CSP based on external resources used
- [ ] Add security commands to `invoke_handler`
- [ ] Set up `SecurityState` in application state
- [ ] Test all security features thoroughly
- [ ] Run `cargo audit` before deployment
- [ ] Review and update dependencies regularly
- [ ] Enable logging for security events
- [ ] Document security decisions in project README

## Security Best Practices

### ✅ DO

1. **Always hash passwords** - Never store plain text
2. **Encrypt sensitive data** - Use AES-256-GCM
3. **Validate all input** - Sanitize and validate
4. **Use CSRF tokens** - For state-changing operations
5. **Rate limit endpoints** - Prevent brute force
6. **Validate file paths** - Prevent directory traversal
7. **Start with minimal permissions** - Add as needed
8. **Use strict CSP** - Relax only when necessary
9. **Log security events** - Monitor for attacks
10. **Update dependencies** - Patch vulnerabilities

### ❌ DON'T

1. **Don't use `'unsafe-eval'` in CSP** - Enables code injection
2. **Don't use `full` capability in production** - Too permissive
3. **Don't store secrets in code** - Use environment variables
4. **Don't ignore rate limiting** - Leaves app vulnerable
5. **Don't trust user input** - Always validate
6. **Don't use MD5/SHA1 for passwords** - Use Argon2id
7. **Don't hardcode encryption keys** - Derive from passwords
8. **Don't expose internal errors** - Sanitize error messages
9. **Don't skip security updates** - Patch promptly
10. **Don't disable HTTPS** - Always use secure connections

## Common Pitfalls

### Pitfall 1: Using `'unsafe-inline'` for Scripts

```json
// ❌ BAD
"csp": "script-src 'self' 'unsafe-inline'"

// ✅ GOOD
"csp": "script-src 'self'"
```

**Solution:** Bundle all scripts, don't use inline scripts.

### Pitfall 2: Not Rate Limiting Login

```rust
// ❌ BAD
#[tauri::command]
async fn login(username: String, password: String) -> Result<()> {
    verify_password(&username, &password)?;
    Ok(())
}

// ✅ GOOD
#[tauri::command]
async fn login(
    username: String,
    password: String,
    state: State<'_, Arc<SecurityState>>,
) -> Result<()> {
    if !state.rate_limiter.check_rate_limit(&username)? {
        return Err(AppError::Authorization("Too many attempts".into()));
    }
    verify_password(&username, &password)?;
    Ok(())
}
```

### Pitfall 3: Not Validating File Paths

```rust
// ❌ BAD
#[tauri::command]
async fn read_file(path: String) -> Result<String> {
    Ok(fs::read_to_string(path)?)
}

// ✅ GOOD
#[tauri::command]
async fn read_file(path: String, allowed_dir: PathBuf) -> Result<String> {
    let safe_path = PathValidator::validate_path(&PathBuf::from(path), &allowed_dir)?;
    Ok(fs::read_to_string(safe_path)?)
}
```

### Pitfall 4: Storing Encryption Keys Insecurely

```rust
// ❌ BAD
let key = "my-secret-key-12345678901234567890"; // Hardcoded!

// ✅ GOOD
let salt = KeyDerivation::generate_salt();
let key_bytes = KeyDerivation::derive_key(user_password, &salt)?;
let encryptor = Encryptor::from_key(&key_bytes)?;
```

## Support and Updates

### Documentation

- Read `SECURITY.md` for complete implementation guide
- Read `CSP.md` for Content Security Policy details
- Check `src-tauri/capabilities/README.md` for permissions

### Getting Help

1. Review example commands in `security_example.rs.tmpl`
2. Check Tauri security documentation: https://tauri.app/security/
3. Review OWASP guidelines: https://owasp.org/

### Updating Templates

When updating security templates:

1. Review new Tauri security features
2. Check for deprecated APIs
3. Update dependency versions
4. Run security audit
5. Update documentation
6. Test thoroughly

## License

These templates are part of the {{PROJECT_NAME}} scaffold project.

---

**Last Updated:** 2026-01-20
**Maintainer:** {{AUTHOR}}
