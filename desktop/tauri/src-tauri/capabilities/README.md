# Tauri Capabilities

Capabilities define the permissions your Tauri application has. Choose the appropriate capability based on your security requirements.

## Available Capabilities

### 1. `default.json.tmpl` - Balanced (Recommended)

**Use for:** Most production applications

**Includes:**
- Window management
- Dialogs (open, save, message)
- File system access (read, write, mkdir, remove)
- HTTP requests
- Persistent storage (store plugin)
- Notifications
- Clipboard access
- OS information
- Auto-updater

**Security Level:** Medium - Allows essential operations with reasonable security

**Configuration:**
```json
"capabilities": ["default"]
```

---

### 2. `minimal.json.tmpl` - Sandboxed (Maximum Security)

**Use for:** Highly sensitive applications, kiosks, or untrusted environments

**Includes:**
- Basic window management (close, minimize, title)
- Simple dialogs (message, confirm)
- Persistent storage
- OS platform info

**Excludes:**
- File system access
- Shell execution
- HTTP requests
- Notifications
- Clipboard

**Security Level:** High - Minimal attack surface

**Configuration:**
```json
"capabilities": ["minimal"]
```

---

### 3. `full.json.tmpl` - Unrestricted (Development Only)

**Use for:** Development and testing ONLY

**Includes:**
- All window controls
- All file system operations
- Shell execution
- All HTTP operations
- Global shortcuts
- Everything else

**Security Level:** Low - ⚠️ **DO NOT USE IN PRODUCTION**

**Configuration:**
```json
"capabilities": ["full"]
```

---

### 4. `custom.json.tmpl` - Customizable Template

**Use for:** Applications with specific security requirements

**Includes:**
- Commented sections for each permission category
- Easy to enable/disable specific features

**Configuration:**
1. Copy `custom.json.tmpl` to `custom.json`
2. Uncomment the permissions you need
3. Remove comments (JSON doesn't support comments in production)

```json
"capabilities": ["custom"]
```

---

## Switching Capabilities

Edit `src-tauri/tauri.conf.json`:

```json
{
  "bundle": {
    "active": true,
    "capabilities": ["default"]  // Change this
  }
}
```

Or use multiple capabilities:

```json
"capabilities": ["minimal", "custom"]
```

## Security Recommendations

### Development
✅ Use `full` or `default` for easier development
✅ Test with production capability before release

### Staging/Testing
✅ Use `default` or `custom`
✅ Verify all features work with production permissions

### Production
✅ Use `minimal` or `custom` with only required permissions
✅ Review permissions regularly
✅ Follow principle of least privilege

## Permission Categories

| Category | Risk Level | Examples |
|----------|------------|----------|
| Window Management | Low | Resize, minimize, maximize |
| Dialogs | Low | Alerts, file pickers |
| OS Info | Low | Platform, version, arch |
| Storage | Low-Medium | Persistent key-value store |
| File System | Medium-High | Read, write, delete files |
| HTTP | Medium | External API calls |
| Clipboard | Medium | Read/write clipboard |
| Notifications | Medium | Show system notifications |
| Shell | High | Execute commands, open URLs |
| Process | High | Exit, restart app |

## Testing Permissions

### 1. Check Runtime Errors

Denied permissions will throw errors:

```javascript
try {
  await invoke('read_file', { path: '/etc/passwd' });
} catch (error) {
  // Error: command read_file not allowed
}
```

### 2. Review Logs

Check Tauri logs for permission warnings:

```bash
cargo tauri dev
```

### 3. Test Before Deployment

Always test your app with production capabilities before release.

## Common Issues

### Issue: Feature not working after switching capability

**Solution:** The feature may require a permission not included in the new capability. Check logs and add the required permission.

### Issue: Too many permissions

**Solution:** Start with `minimal` and add permissions as needed, rather than starting with `full` and removing.

### Issue: App crashes when changing capabilities

**Solution:** Ensure capability file is valid JSON and all referenced permissions exist.

## Best Practices

1. **Start Restrictive:** Begin with `minimal` and add permissions as needed
2. **Document Reasons:** Comment why each permission is required
3. **Regular Audits:** Review permissions quarterly
4. **Security Testing:** Test with production capabilities before release
5. **User Privacy:** Only request permissions that are essential

## Further Reading

- [Tauri Security Documentation](https://tauri.app/security/)
- [Tauri Capability System](https://tauri.app/v2/reference/capabilities/)
- [OWASP Least Privilege](https://owasp.org/www-community/Access_Control)

---

**Last Updated:** 2026-01-20
