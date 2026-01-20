# {{PROJECT_NAME}} - Angular Frontend

Modern Angular 17+ frontend application with standalone components, signals, and Tailwind CSS.

## Features

- **Angular 17+** with standalone components
- **Signals** for reactive state management
- **NgRx Signal Store** for global state
- **New Control Flow** syntax (@if, @for, @switch)
- **Tailwind CSS** for styling
- **JWT Authentication** with refresh tokens
- **Route Guards** and interceptors
- **Lazy Loading** for optimal performance
- **Multi-stage Docker** build with nginx
- **Unit Testing** with Karma/Jasmine
- **TypeScript** with strict mode
- **Path Aliases** for clean imports

## Tech Stack

| Category | Technology |
|----------|------------|
| Framework | Angular 17+ |
| State Management | NgRx Signals |
| Styling | Tailwind CSS |
| HTTP Client | Angular HttpClient |
| Routing | Angular Router |
| Forms | Angular Reactive Forms |
| Testing | Karma + Jasmine |
| Build Tool | Angular CLI |
| Package Manager | npm |

## Project Structure

```
src/
├── app/
│   ├── core/                    # Core module (singleton services)
│   │   ├── guards/              # Route guards
│   │   ├── interceptors/        # HTTP interceptors
│   │   └── services/            # Core services
│   ├── shared/                  # Shared module (reusable components)
│   │   ├── components/          # UI components
│   │   ├── directives/          # Custom directives
│   │   └── pipes/               # Custom pipes
│   ├── features/                # Feature modules
│   │   ├── auth/                # Authentication
│   │   ├── dashboard/           # Dashboard
│   │   └── users/               # User management
│   ├── store/                   # NgRx Signal Stores
│   ├── app.component.ts         # Root component
│   ├── app.config.ts            # App configuration
│   └── app.routes.ts            # Route configuration
├── environments/                # Environment configurations
├── assets/                      # Static assets
└── styles.css                   # Global styles
```

## Prerequisites

- Node.js >= 18.x
- npm >= 9.x
- Angular CLI >= 17.x

## Installation

```bash
# Install Angular CLI globally
npm install -g @angular/cli

# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Update environment variables
# Edit .env with your API configuration
```

## Development

```bash
# Start development server
npm start

# Application will be available at http://localhost:4200

# Run with specific host/port
ng serve --host 0.0.0.0 --port 4200
```

## Building

```bash
# Development build
npm run build

# Production build
npm run build:prod

# Build output will be in dist/{{PROJECT_NAME}}/
```

## Testing

```bash
# Run unit tests
npm test

# Run tests with coverage
npm run test:ci

# Coverage reports will be in coverage/{{PROJECT_NAME}}/
```

## Code Quality

```bash
# Lint code
npm run lint

# Format code with Prettier
npm run format

# Check formatting
npm run format:check
```

## Docker

### Build Image

```bash
# Build production image
docker build -t {{PROJECT_NAME}}-frontend .

# Build with specific tag
docker build -t {{PROJECT_NAME}}-frontend:1.0.0 .
```

### Run Container

```bash
# Run container
docker run -p 80:80 {{PROJECT_NAME}}-frontend

# Run with environment variables
docker run -p 80:80 \
  -e API_URL=https://api.example.com \
  {{PROJECT_NAME}}-frontend

# Application will be available at http://localhost
```

### Docker Compose

```bash
# Start services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

## Path Aliases

The project uses TypeScript path aliases for clean imports:

```typescript
// Instead of
import { AuthService } from '../../../core/services/auth.service';

// Use
import { AuthService } from '@core/services/auth.service';
```

Available aliases:
- `@app/*` - src/app/*
- `@core/*` - src/app/core/*
- `@shared/*` - src/app/shared/*
- `@features/*` - src/app/features/*
- `@store/*` - src/app/store/*
- `@environments/*` - src/environments/*

## Authentication

The application includes a complete JWT authentication system:

### Login

```typescript
// Login with credentials
authService.login({ email, password }).subscribe({
  next: (response) => {
    // Token stored automatically
    // Redirected to dashboard
  },
  error: (error) => {
    // Handle error
  }
});
```

### Protected Routes

Routes are protected using the `authGuard`:

```typescript
{
  path: 'dashboard',
  component: DashboardComponent,
  canActivate: [authGuard]
}
```

### Token Management

Tokens are automatically:
- Stored in localStorage
- Attached to requests via interceptor
- Checked for expiry
- Refreshed when needed

## State Management

The application uses NgRx Signal Store for state management:

```typescript
// Inject store
private authStore = inject(AuthStore);

// Read state
const user = this.authStore.user();
const isLoggedIn = this.authStore.isLoggedIn();

// Update state
this.authStore.setUser(user);
this.authStore.logout();
```

## Components

### Button Component

```html
<app-button
  variant="primary"
  size="md"
  [loading]="isLoading"
  (clicked)="handleClick()"
>
  Click Me
</app-button>
```

### Input Component

```html
<app-input
  id="email"
  type="email"
  label="Email"
  placeholder="you@example.com"
  [required]="true"
  [(ngModel)]="email"
/>
```

### Card Component

```html
<app-card title="Card Title" [action]="true">
  <button card-action>Action</button>
  <p>Card content</p>
  <div card-footer>Footer content</div>
</app-card>
```

## Environment Variables

### Development (.env)

```env
NG_APP_API_URL=http://localhost:8000/api/v1
NG_APP_API_TIMEOUT=30000
NG_APP_ENABLE_ANALYTICS=false
```

### Production

Set these via Docker environment variables or CI/CD:

```bash
API_URL=https://api.example.com
```

## Deployment

### Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Production deployment
vercel --prod
```

### Netlify

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=dist/{{PROJECT_NAME}}/browser
```

### AWS S3 + CloudFront

```bash
# Build production
npm run build:prod

# Sync to S3
aws s3 sync dist/{{PROJECT_NAME}}/browser s3://your-bucket --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id YOUR_ID --paths "/*"
```

## Performance Optimization

- **Lazy Loading**: All feature modules are lazy loaded
- **Code Splitting**: Automatic via Angular CLI
- **Tree Shaking**: Unused code removed in production
- **AOT Compilation**: Enabled by default
- **Minification**: All assets minified in production
- **Gzip Compression**: Enabled in nginx
- **Browser Caching**: Static assets cached for 1 year

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Troubleshooting

### Port Already in Use

```bash
# Kill process on port 4200
npx kill-port 4200

# Or use different port
ng serve --port 4300
```

### Module Not Found

```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Build Errors

```bash
# Clear Angular cache
rm -rf .angular/cache
ng build
```

## Contributing

1. Create feature branch: `git checkout -b feature/my-feature`
2. Make changes and test
3. Run linting: `npm run lint`
4. Commit changes: `git commit -m "feat: add my feature"`
5. Push branch: `git push origin feature/my-feature`
6. Create Pull Request

## License

MIT

## Author

{{AUTHOR}}

---

**Version:** 1.0.0
**Last Updated:** 2025-01-19
