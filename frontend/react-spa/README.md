# React SPA Templates

> Modern React Single Page Application templates with Vite
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for building modern React SPAs with:

- **Build Tool**: Vite (fast HMR, optimized builds)
- **State Management**: Zustand (lightweight, TypeScript-first)
- **Data Fetching**: TanStack Query (React Query v5)
- **Routing**: React Router v6
- **Styling**: Tailwind CSS + shadcn/ui
- **Forms**: React Hook Form + Zod
- **Auth**: JWT with refresh tokens
- **API Client**: Axios with interceptors
- **Testing**: Vitest + React Testing Library
- **Deployment**: Docker + Nginx

---

## Directory Structure

```
frontend/react-spa/
├── src/
│   ├── components/              # Reusable components
│   │   ├── ui/                   # shadcn/ui components
│   │   ├── layouts/
│   │   │   ├── AppLayout.tsx.tmpl
│   │   │   └── AuthLayout.tsx.tmpl
│   │   └── shared/
│   │       ├── Navbar.tsx.tmpl
│   │       └── Footer.tsx.tmpl
│   │
│   ├── features/                # Feature-based modules
│   │   ├── auth/
│   │   │   ├── components/
│   │   │   │   ├── LoginForm.tsx.tmpl
│   │   │   │   └── RegisterForm.tsx.tmpl
│   │   │   ├── hooks/
│   │   │   │   └── useAuth.ts.tmpl
│   │   │   └── api/
│   │   │       └── auth.ts.tmpl
│   │   │
│   │   └── users/
│   │       ├── components/
│   │       ├── hooks/
│   │       └── api/
│   │
│   ├── lib/                     # Library configurations
│   │   ├── api.ts.tmpl           # Axios instance
│   │   ├── query-client.ts.tmpl  # React Query setup
│   │   └── utils.ts.tmpl         # Utilities
│   │
│   ├── stores/                  # Zustand stores
│   │   ├── auth.ts.tmpl
│   │   └── ui.ts.tmpl
│   │
│   ├── routes/                  # Route definitions
│   │   ├── index.tsx.tmpl
│   │   ├── ProtectedRoute.tsx.tmpl
│   │   └── routes.tsx.tmpl
│   │
│   ├── pages/                   # Page components
│   │   ├── Home.tsx.tmpl
│   │   ├── Login.tsx.tmpl
│   │   ├── Dashboard.tsx.tmpl
│   │   └── NotFound.tsx.tmpl
│   │
│   ├── hooks/                   # Custom hooks
│   │   ├── useDebounce.ts.tmpl
│   │   └── useLocalStorage.ts.tmpl
│   │
│   ├── types/                   # TypeScript types
│   │   ├── api.ts.tmpl
│   │   └── models.ts.tmpl
│   │
│   ├── App.tsx.tmpl
│   ├── main.tsx.tmpl
│   └── index.css.tmpl
│
├── public/
│   └── favicon.ico
│
├── tests/
│   ├── setup.ts.tmpl
│   └── utils.tsx.tmpl
│
├── .env.example.tmpl
├── vite.config.ts.tmpl
├── tailwind.config.js.tmpl
├── postcss.config.js.tmpl
├── tsconfig.json.tmpl
├── package.json.tmpl
├── Dockerfile.tmpl
└── nginx.conf.tmpl
```

---

## Quick Start

### 1. Setup Project

```bash
# Copy templates
mkdir -p apps/web
cp -r frontend/react-spa/* apps/web/

# Install dependencies
cd apps/web
npm install
```

### 2. Configure Environment

```bash
# .env
cat > .env <<EOF
VITE_API_URL=http://localhost:8000/api
VITE_APP_NAME=My App
VITE_APP_ENV=development
EOF
```

### 3. Run Development Server

```bash
# Start dev server
npm run dev

# Open browser
open http://localhost:5173
```

### 4. Build for Production

```bash
# Build
npm run build

# Preview build
npm run preview

# Type check
npm run type-check

# Lint
npm run lint
```

---

## Template Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-react-app` |
| `{{AUTHOR}}` | Author name | `Your Name` |
| `{{API_URL}}` | API base URL | `http://localhost:8000` |
| `{{APP_NAME}}` | Application name | `My App` |

---

## Tech Stack Details

### Core Dependencies

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",
    "@tanstack/react-query": "^5.14.0",
    "zustand": "^4.4.7",
    "axios": "^1.6.2",
    "react-hook-form": "^7.48.2",
    "zod": "^3.22.4",
    "@hookform/resolvers": "^3.3.2"
  },
  "devDependencies": {
    "vite": "^5.0.0",
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.3.3",
    "tailwindcss": "^3.3.6",
    "vitest": "^1.0.4",
    "@testing-library/react": "^14.1.2"
  }
}
```

---

## State Management (Zustand)

### Auth Store

```typescript
// stores/auth.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface User {
  id: string;
  email: string;
  name: string;
}

interface AuthState {
  user: User | null;
  token: string | null;
  isAuthenticated: boolean;
  login: (token: string, user: User) => void;
  logout: () => void;
  updateUser: (user: Partial<User>) => void;
}

export const useAuthStore = create<AuthState>()(
  persist(
    (set) => ({
      user: null,
      token: null,
      isAuthenticated: false,

      login: (token, user) =>
        set({ token, user, isAuthenticated: true }),

      logout: () =>
        set({ token: null, user: null, isAuthenticated: false }),

      updateUser: (userData) =>
        set((state) => ({
          user: state.user ? { ...state.user, ...userData } : null,
        })),
    }),
    {
      name: 'auth-storage',
    }
  )
);
```

### UI Store

```typescript
// stores/ui.ts
import { create } from 'zustand';

interface UIState {
  isSidebarOpen: boolean;
  theme: 'light' | 'dark';
  toggleSidebar: () => void;
  setTheme: (theme: 'light' | 'dark') => void;
}

export const useUIStore = create<UIState>((set) => ({
  isSidebarOpen: true,
  theme: 'light',

  toggleSidebar: () =>
    set((state) => ({ isSidebarOpen: !state.isSidebarOpen })),

  setTheme: (theme) => set({ theme }),
}));
```

---

## Data Fetching (React Query)

### API Client

```typescript
// lib/api.ts
import axios from 'axios';
import { useAuthStore } from '@/stores/auth';

export const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - add auth token
api.interceptors.request.use((config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor - handle errors
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Token expired - logout
      useAuthStore.getState().logout();
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

### Query Client Setup

```typescript
// lib/query-client.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 1,
      refetchOnWindowFocus: false,
      staleTime: 5 * 60 * 1000, // 5 minutes
    },
  },
});
```

### Custom Hooks

```typescript
// features/users/hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { api } from '@/lib/api';
import { User } from '@/types/models';

export function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: async () => {
      const { data } = await api.get<User[]>('/users');
      return data;
    },
  });
}

export function useUser(id: string) {
  return useQuery({
    queryKey: ['users', id],
    queryFn: async () => {
      const { data } = await api.get<User>(`/users/${id}`);
      return data;
    },
    enabled: !!id,
  });
}

export function useCreateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (userData: Partial<User>) => {
      const { data } = await api.post<User>('/users', userData);
      return data;
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
}

export function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async ({ id, ...userData }: Partial<User> & { id: string }) => {
      const { data } = await api.put<User>(`/users/${id}`, userData);
      return data;
    },
    onSuccess: (data) => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
      queryClient.invalidateQueries({ queryKey: ['users', data.id] });
    },
  });
}
```

---

## Routing (React Router)

### Route Configuration

```typescript
// routes/routes.tsx
import { createBrowserRouter } from 'react-router-dom';
import { AppLayout } from '@/components/layouts/AppLayout';
import { AuthLayout } from '@/components/layouts/AuthLayout';
import { ProtectedRoute } from './ProtectedRoute';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <AppLayout />,
    children: [
      {
        index: true,
        element: <Home />,
      },
      {
        path: 'dashboard',
        element: (
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        ),
      },
    ],
  },
  {
    path: '/auth',
    element: <AuthLayout />,
    children: [
      {
        path: 'login',
        element: <Login />,
      },
      {
        path: 'register',
        element: <Register />,
      },
    ],
  },
  {
    path: '*',
    element: <NotFound />,
  },
]);
```

### Protected Route

```typescript
// routes/ProtectedRoute.tsx
import { Navigate } from 'react-router-dom';
import { useAuthStore } from '@/stores/auth';

export function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);

  if (!isAuthenticated) {
    return <Navigate to="/auth/login" replace />;
  }

  return <>{children}</>;
}
```

---

## Forms (React Hook Form + Zod)

### Login Form

```typescript
// features/auth/components/LoginForm.tsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useNavigate } from 'react-router-dom';
import { useAuthStore } from '@/stores/auth';
import { useLogin } from '../hooks/useAuth';

const loginSchema = z.object({
  email: z.string().email('Invalid email address'),
  password: z.string().min(8, 'Password must be at least 8 characters'),
});

type LoginFormData = z.infer<typeof loginSchema>;

export function LoginForm() {
  const navigate = useNavigate();
  const login = useAuthStore((state) => state.login);
  const { mutate, isLoading } = useLogin();

  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<LoginFormData>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = (data: LoginFormData) => {
    mutate(data, {
      onSuccess: (response) => {
        login(response.token, response.user);
        navigate('/dashboard');
      },
    });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div>
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          {...register('email')}
          className="w-full px-4 py-2 border rounded"
        />
        {errors.email && (
          <p className="text-red-500 text-sm">{errors.email.message}</p>
        )}
      </div>

      <div>
        <label htmlFor="password">Password</label>
        <input
          id="password"
          type="password"
          {...register('password')}
          className="w-full px-4 py-2 border rounded"
        />
        {errors.password && (
          <p className="text-red-500 text-sm">{errors.password.message}</p>
        )}
      </div>

      <button
        type="submit"
        disabled={isLoading}
        className="w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600"
      >
        {isLoading ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

---

## Styling (Tailwind + shadcn/ui)

### Tailwind Config

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#f0f9ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        },
      },
    },
  },
  plugins: [],
};
```

### shadcn/ui Integration

```bash
# Install shadcn/ui
npx shadcn-ui@latest init

# Add components
npx shadcn-ui@latest add button
npx shadcn-ui@latest add input
npx shadcn-ui@latest add dialog
```

---

## Testing

### Vitest Config

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './tests/setup.ts',
  },
});
```

### Test Example

```typescript
// features/auth/components/LoginForm.test.tsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { LoginForm } from './LoginForm';
import { QueryClientProvider } from '@tanstack/react-query';
import { queryClient } from '@/lib/query-client';

describe('LoginForm', () => {
  it('should validate email and password', async () => {
    render(
      <QueryClientProvider client={queryClient}>
        <LoginForm />
      </QueryClientProvider>
    );

    const submitButton = screen.getByRole('button', { name: /login/i });
    await userEvent.click(submitButton);

    await waitFor(() => {
      expect(screen.getByText(/invalid email/i)).toBeInTheDocument();
    });
  });

  it('should submit valid form', async () => {
    const user = userEvent.setup();
    render(
      <QueryClientProvider client={queryClient}>
        <LoginForm />
      </QueryClientProvider>
    );

    await user.type(screen.getByLabelText(/email/i), 'test@example.com');
    await user.type(screen.getByLabelText(/password/i), 'password123');
    await user.click(screen.getByRole('button', { name: /login/i }));

    // Assert API call was made
  });
});
```

---

## Deployment

### Docker

```dockerfile
# Dockerfile
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Production
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Nginx Config

```nginx
# nginx.conf
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # API proxy
    location /api {
        proxy_pass http://api:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    gzip on;
    gzip_types text/plain text/css application/json application/javascript;
}
```

---

## Best Practices

### Code Organization

- Feature-based folders
- Colocate related files
- Barrel exports for clean imports

### Performance

- Lazy load routes
- Code splitting
- Memoization (useMemo, useCallback)
- Virtual scrolling for long lists

### Accessibility

- Semantic HTML
- ARIA labels
- Keyboard navigation
- Focus management

### Security

- Sanitize user input
- HTTPS only in production
- Secure token storage
- XSS protection

---

## Resources

### Documentation

- [Vite](https://vitejs.dev/)
- [React Router](https://reactrouter.com/)
- [TanStack Query](https://tanstack.com/query/)
- [Zustand](https://zustand-demo.pmnd.rs/)
- [React Hook Form](https://react-hook-form.com/)
- [Tailwind CSS](https://tailwindcss.com/)

---

## Support

For issues or questions:
1. Check the documentation
2. Review examples
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
