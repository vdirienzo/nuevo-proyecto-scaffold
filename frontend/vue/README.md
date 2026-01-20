# {{PROJECT_NAME}} - Vue 3 Frontend

A modern, production-ready Vue 3 frontend application built with TypeScript, Composition API, Pinia, Vue Router, and Tailwind CSS.

## Author

{{AUTHOR}}

---

## Features

- ⚡ **Vue 3** with Composition API and `<script setup>`
- 🎨 **Tailwind CSS** for modern, responsive styling
- 📦 **Pinia** for state management with persistence
- 🛣️ **Vue Router** with authentication guards
- 🔐 **JWT Authentication** with refresh token support
- 🧪 **Vitest** for unit testing
- 📝 **TypeScript** for type safety
- 🚀 **Vite** for fast development and optimized builds
- 🐳 **Docker** ready with multi-stage builds
- 🎯 **Reusable UI Components** (Button, Input, Card, Modal)
- 📱 **Responsive Layout** with sidebar navigation
- 🔄 **API Integration** with Axios and interceptors
- ✅ **Form Validation** with custom validators
- 🎭 **Composables** for reusable logic

---

## Project Structure

```
vue/
├── public/                   # Static assets
├── src/
│   ├── assets/
│   │   └── css/
│   │       └── main.css      # Global styles with Tailwind
│   ├── components/
│   │   ├── layout/           # Layout components
│   │   │   ├── AppHeader.vue
│   │   │   ├── AppSidebar.vue
│   │   │   └── AppLayout.vue
│   │   └── ui/               # Reusable UI components
│   │       ├── VButton.vue
│   │       ├── VInput.vue
│   │       ├── VCard.vue
│   │       └── VModal.vue
│   ├── composables/          # Composition functions
│   │   ├── useAuth.ts
│   │   ├── useApi.ts
│   │   ├── useNotification.ts
│   │   └── useForm.ts
│   ├── router/               # Vue Router configuration
│   │   └── index.ts
│   ├── services/             # API services
│   │   ├── api.ts            # Axios client
│   │   └── auth.ts           # Auth API calls
│   ├── stores/               # Pinia stores
│   │   ├── auth.ts
│   │   ├── users.ts
│   │   └── ui.ts
│   ├── types/                # TypeScript types
│   │   ├── index.ts
│   │   └── api.ts
│   ├── utils/                # Utility functions
│   │   ├── helpers.ts
│   │   └── validators.ts
│   ├── views/                # Page components
│   │   ├── HomeView.vue
│   │   ├── LoginView.vue
│   │   ├── RegisterView.vue
│   │   ├── DashboardView.vue
│   │   ├── UsersView.vue
│   │   └── NotFoundView.vue
│   ├── App.vue               # Root component
│   └── main.ts               # Application entry point
├── .env.example              # Environment variables template
├── Dockerfile                # Multi-stage Docker build
├── nginx.conf                # Nginx configuration
├── index.html                # HTML entry point
├── vite.config.ts            # Vite configuration
├── vitest.config.ts          # Vitest configuration
├── tailwind.config.js        # Tailwind CSS configuration
├── tsconfig.json             # TypeScript configuration
└── package.json              # Project dependencies
```

---

## Getting Started

### Prerequisites

- Node.js 20+
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Copy environment variables
cp .env.example .env

# Update .env with your API URL
VITE_API_URL=http://localhost:8000
```

### Development

```bash
# Start dev server (http://localhost:3000)
npm run dev

# Run tests
npm run test

# Run tests with UI
npm run test:ui

# Run tests with coverage
npm run test:coverage

# Lint code
npm run lint

# Format code
npm run format
```

### Build

```bash
# Build for production
npm run build

# Preview production build
npm run preview
```

### Docker

```bash
# Build image
docker build -t {{PROJECT_NAME}}-frontend .

# Run container
docker run -p 80:80 {{PROJECT_NAME}}-frontend
```

---

## Configuration

### Environment Variables

Create a `.env` file based on `.env.example`:

| Variable | Description | Default |
|----------|-------------|---------|
| `VITE_API_URL` | Backend API URL | `http://localhost:8000` |
| `VITE_API_TIMEOUT` | API request timeout (ms) | `30000` |
| `VITE_APP_NAME` | Application name | `{{PROJECT_NAME}}` |
| `VITE_JWT_TOKEN_KEY` | LocalStorage key for token | `auth_token` |
| `VITE_JWT_REFRESH_KEY` | LocalStorage key for refresh token | `refresh_token` |

---

## Key Features

### Authentication

- JWT-based authentication with access and refresh tokens
- Automatic token refresh on 401 responses
- Protected routes with navigation guards
- Persistent auth state with localStorage

### State Management

- **Auth Store**: User authentication and profile
- **Users Store**: User CRUD operations
- **UI Store**: Sidebar, theme, and modal state

All stores use Pinia with persistence plugin.

### UI Components

#### VButton
```vue
<VButton
  variant="primary"
  size="md"
  :loading="false"
  :disabled="false"
  full-width
  @click="handleClick"
>
  Click Me
</VButton>
```

#### VInput
```vue
<VInput
  v-model="formData.email"
  label="Email"
  type="email"
  :error="errors.email"
  required
  @blur="handleBlur"
/>
```

#### VCard
```vue
<VCard title="Card Title" padding="md" hover>
  <p>Card content</p>
  <template #footer>
    <VButton>Action</VButton>
  </template>
</VCard>
```

#### VModal
```vue
<VModal v-model="isOpen" title="Modal Title" size="md">
  <p>Modal content</p>
  <template #footer>
    <VButton @click="isOpen = false">Close</VButton>
  </template>
</VModal>
```

### Composables

#### useAuth
```typescript
const { login, logout, isAuthenticated, user } = useAuth()

await login({ email, password })
```

#### useApi
```typescript
const { data, loading, error, execute } = useApi<User[]>('/users')

await execute()
```

#### useForm
```typescript
const { formData, errors, handleSubmit } = useForm({
  email: {
    initialValue: '',
    rules: [required(), email()]
  }
})

await handleSubmit(async (data) => {
  // Submit logic
})
```

#### useNotification
```typescript
const { showSuccess, showError } = useNotification()

showSuccess('Operation successful')
showError('Something went wrong')
```

### Validators

Available validation rules:

- `required(message?)` - Field is required
- `email(message?)` - Valid email format
- `minLength(min, message?)` - Minimum character length
- `maxLength(max, message?)` - Maximum character length
- `pattern(regex, message?)` - Regex pattern match
- `isNumber(message?)` - Must be numeric
- `min(value, message?)` - Minimum numeric value
- `max(value, message?)` - Maximum numeric value
- `matchField(field, message?)` - Match another field (password confirmation)
- `url(message?)` - Valid URL
- `phone(message?)` - Valid phone number
- `alphanumeric(message?)` - Only letters and numbers
- `custom(fn, message?)` - Custom validation function

---

## API Integration

### Axios Client

The project uses Axios with interceptors for:

- Automatic JWT token injection
- Automatic token refresh on 401
- Global error handling
- Request/response logging

```typescript
import { apiClient } from '@/services/api'

// GET request
const { data } = await apiClient.get<User[]>('/users')

// POST request
const { data } = await apiClient.post<User>('/users', userData)

// PUT request
await apiClient.put(`/users/${id}`, userData)

// DELETE request
await apiClient.delete(`/users/${id}`)
```

---

## Testing

Tests are written using Vitest and Vue Test Utils.

### Run Tests

```bash
# Run all tests
npm run test

# Watch mode
npm run test -- --watch

# Coverage report
npm run test:coverage

# UI mode
npm run test:ui
```

### Example Test

```typescript
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import VButton from '@/components/ui/VButton.vue'

describe('VButton', () => {
  it('renders correctly', () => {
    const wrapper = mount(VButton, {
      slots: { default: 'Click me' }
    })
    expect(wrapper.text()).toBe('Click me')
  })
})
```

---

## Deployment

### Production Build

```bash
npm run build
```

Output: `dist/` directory

### Docker Deployment

```bash
# Build image
docker build -t {{PROJECT_NAME}}-frontend .

# Run container
docker run -d -p 80:80 {{PROJECT_NAME}}-frontend
```

The Docker image uses:
- Node 20 Alpine for building
- Nginx Alpine for serving
- Multi-stage build for optimal size
- Health check endpoint at `/health`

### Nginx Configuration

The included `nginx.conf` provides:

- SPA routing (serves `index.html` for all routes)
- Gzip compression
- Security headers
- Static asset caching (1 year)
- Health check endpoint

---

## Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)

---

## Contributing

1. Follow Vue 3 Composition API style guide
2. Use TypeScript for all new code
3. Write tests for new components
4. Use ESLint and Prettier
5. Follow commit message conventions

---

## Troubleshooting

### Common Issues

**Issue**: `npm install` fails
**Solution**: Clear cache with `npm cache clean --force` and retry

**Issue**: Vite dev server not starting
**Solution**: Check port 3000 is available, or change port in `vite.config.ts`

**Issue**: API requests failing with CORS
**Solution**: Ensure backend allows requests from `http://localhost:3000`

**Issue**: TypeScript errors in IDE
**Solution**: Restart TypeScript server or run `npm run build` to check for errors

---

## License

Private project - All rights reserved

---

## Changelog

### [1.0.0] - Initial Release

#### Added
- Vue 3 with Composition API setup
- Pinia state management with persistence
- Vue Router with authentication guards
- JWT authentication with refresh tokens
- Reusable UI components (Button, Input, Card, Modal)
- Form validation with custom validators
- Axios API client with interceptors
- Tailwind CSS for styling
- Vitest for testing
- Docker support with Nginx
- Complete TypeScript types
- Responsive layout with sidebar
- User management views

---

**Built with ❤️ by {{AUTHOR}}**
