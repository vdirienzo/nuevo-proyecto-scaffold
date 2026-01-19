# Authentication Provider Templates

Integration templates for various authentication providers and authorization systems.

**Author:** {{AUTHOR}}
**Project:** {{PROJECT_NAME}}

---

## Directory Structure

```
auth/
├── auth0/                      # Auth0 integration (7 files)
│   ├── config.ts.tmpl
│   ├── client.ts.tmpl
│   ├── middleware.ts.tmpl
│   ├── api/
│   │   ├── callback.ts.tmpl
│   │   ├── login.ts.tmpl
│   │   └── logout.ts.tmpl
│   └── hooks/
│       └── use-auth0.ts.tmpl
│
├── clerk/                      # Clerk integration (5 files)
│   ├── config.ts.tmpl
│   ├── middleware.ts.tmpl
│   ├── components/
│   │   ├── user-button.tsx.tmpl
│   │   └── sign-in.tsx.tmpl
│   └── hooks/
│       └── use-clerk.ts.tmpl
│
├── saml/                       # SAML SSO (4 files)
│   ├── config.ts.tmpl
│   ├── passport-saml.ts.tmpl
│   ├── metadata.xml.tmpl
│   └── routes/
│       └── saml.ts.tmpl
│
├── mfa/                        # Multi-Factor Authentication (6 files)
│   ├── totp/
│   │   ├── setup.ts.tmpl
│   │   ├── verify.ts.tmpl
│   │   └── qr-code.tsx.tmpl
│   ├── sms/
│   │   └── twilio.ts.tmpl
│   └── backup-codes/
│       ├── generate.ts.tmpl
│       └── verify.ts.tmpl
│
└── authorization/              # RBAC/ABAC (6 files)
    ├── rbac/
    │   ├── roles.ts.tmpl
    │   ├── permissions.ts.tmpl
    │   └── guard.ts.tmpl
    ├── abac/
    │   ├── policies.ts.tmpl
    │   └── evaluator.ts.tmpl
    └── decorators/
        └── require-permission.ts.tmpl
```

---

## Auth0 Integration

Complete Auth0 authentication setup with React SDK and Next.js middleware.

### Files:
- **config.ts**: Auth0 configuration and environment variables
- **client.ts**: Auth0Provider wrapper for React
- **middleware.ts**: Next.js middleware for route protection
- **api/callback.ts**: OAuth callback handler
- **api/login.ts**: Login redirect handler
- **api/logout.ts**: Logout handler
- **hooks/use-auth0.ts**: Custom React hook with helpers

### Dependencies:
```json
{
  "@auth0/auth0-react": "^2.2.4",
  "jose": "^5.2.0"
}
```

---

## Clerk Integration

Clerk authentication with pre-built components and middleware.

### Files:
- **config.ts**: Clerk configuration
- **middleware.ts**: authMiddleware with custom logic
- **components/user-button.tsx**: User button with dropdown
- **components/sign-in.tsx**: Customized sign-in page
- **hooks/use-clerk.ts**: Custom hook with additional utilities

### Dependencies:
```json
{
  "@clerk/nextjs": "^4.29.0"
}
```

---

## SAML SSO

Enterprise SAML 2.0 Single Sign-On integration.

### Files:
- **config.ts**: SAML configuration (IdP, SP, certificates)
- **passport-saml.ts**: Passport SAML strategy
- **metadata.xml**: Service Provider metadata template
- **routes/saml.ts**: Login, callback, logout, and metadata endpoints

### Dependencies:
```json
{
  "passport": "^0.7.0",
  "passport-saml": "^3.2.4",
  "express": "^4.18.2"
}
```

---

## Multi-Factor Authentication (MFA)

Complete MFA system with TOTP, SMS, and backup codes.

### TOTP (Time-based One-Time Password)
- **setup.ts**: Generate TOTP secret and QR code
- **verify.ts**: Verify TOTP tokens
- **qr-code.tsx**: React component for setup flow

### SMS Verification
- **twilio.ts**: Twilio SMS verification integration

### Backup Codes
- **generate.ts**: Generate and hash backup codes
- **verify.ts**: Verify backup codes

### Dependencies:
```json
{
  "speakeasy": "^2.0.0",
  "qrcode": "^1.5.3",
  "twilio": "^4.20.1",
  "bcrypt": "^5.1.1"
}
```

---

## Authorization (RBAC/ABAC)

Role-Based and Attribute-Based Access Control systems.

### RBAC (Role-Based Access Control)
- **roles.ts**: Role definitions and hierarchy
- **permissions.ts**: Permission rules and checks
- **guard.ts**: Middleware guards for routes

### ABAC (Attribute-Based Access Control)
- **policies.ts**: Policy definitions
- **evaluator.ts**: Policy evaluation engine

### Decorators
- **require-permission.ts**: Decorators and HOCs for permission checks

### Usage:
```typescript
// RBAC
import { requireRole } from './authorization/rbac/guard';
const handler = requireRole(Role.ADMIN)(async (req, res) => {
  // Handler code
});

// ABAC
import { PolicyEvaluator } from './authorization/abac/evaluator';
const evaluator = new PolicyEvaluator(policies);
const result = evaluator.evaluate(context);
```

---

## Template Variables

All templates use the following placeholders:

| Variable | Description |
|----------|-------------|
| `{{AUTHOR}}` | Project author name |
| `{{PROJECT_NAME}}` | Project name |
| `{{SAML_ISSUER}}` | SAML issuer (SP entity ID) |
| `{{SAML_CERT}}` | SAML certificate (base64) |
| `{{SAML_CALLBACK_URL}}` | SAML callback URL |
| `{{SAML_LOGOUT_URL}}` | SAML logout URL |
| `{{APP_URL}}` | Application base URL |
| `{{SUPPORT_EMAIL}}` | Support email address |

---

## Environment Variables

### Auth0
```env
AUTH0_DOMAIN=your-domain.auth0.com
AUTH0_CLIENT_ID=your_client_id
AUTH0_CLIENT_SECRET=your_client_secret
AUTH0_AUDIENCE=https://your-api.com
AUTH0_REDIRECT_URI=http://localhost:3000/api/auth/callback
AUTH0_POST_LOGOUT_REDIRECT_URI=http://localhost:3000
```

### Clerk
```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/dashboard
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/onboarding
```

### SAML
```env
SAML_ENTRY_POINT=https://idp.example.com/sso
SAML_ISSUER=urn:example:sp
SAML_CALLBACK_URL=http://localhost:3000/api/auth/saml/callback
SAML_IDP_CERT=MIIDXTCCAkWgAwIBAgIJAL...
SAML_CERT=MIIDXTCCAkWgAwIBAgIJAL...
SAML_PRIVATE_KEY=-----BEGIN PRIVATE KEY-----...
```

### MFA
```env
# Twilio SMS
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=xxxxxxxxxxxxxxxxx
TWILIO_VERIFY_SERVICE_SID=VAxxxxxxxxxxxxxxxxx
```

---

## Usage Examples

### Auth0
```typescript
import { useAuth0 } from './auth/auth0/hooks/use-auth0';

function MyComponent() {
  const { isAuthenticated, user, loginWithRedirect, logout } = useAuth0();

  if (!isAuthenticated) {
    return <button onClick={() => loginWithRedirect()}>Login</button>;
  }

  return <button onClick={logout}>Logout</button>;
}
```

### Clerk
```typescript
import { useClerk } from './auth/clerk/hooks/use-clerk';

function MyComponent() {
  const { isAuthenticated, user, isReady } = useClerk();

  if (!isReady) return <div>Loading...</div>;

  return <div>Hello {user?.firstName}</div>;
}
```

### RBAC
```typescript
import { requirePermission } from './auth/authorization/rbac/guard';
import { Resource, Action } from './auth/authorization/rbac/permissions';

const handler = requirePermission(
  `${Resource.POST}:${Action.UPDATE}`,
  (req) => ({ resourceOwnerId: req.query.userId })
)(async (req, res) => {
  // Protected handler
});
```

### MFA TOTP
```typescript
import { setupTOTP } from './auth/mfa/totp/setup';
import { verifyTOTP } from './auth/mfa/totp/verify';

// Setup
const { secret, qrCodeDataUrl, backupCodes } = await setupTOTP(
  userId,
  email,
  'MyApp'
);

// Verify
const result = verifyTOTP(token, secret);
if (result.valid) {
  // Token is valid
}
```

---

## Statistics

- **Total Files**: 28
- **Total Lines**: ~1,908
- **Auth0**: 286 lines
- **Clerk**: 159 lines
- **SAML**: 257 lines
- **MFA**: 517 lines
- **Authorization**: 689 lines

---

## License

These templates are part of the {{PROJECT_NAME}} project.

**Author:** {{AUTHOR}}
