# {{PROJECT_NAME}} - jQuery Frontend

Modern jQuery frontend application with best practices, modular architecture, and production-ready features.

## Features

- **Modern jQuery 3.7+** - Latest stable version
- **Bootstrap 5** - Responsive UI framework
- **Parcel Bundler** - Zero-config bundler
- **ES6+ Modules** - Modern JavaScript patterns
- **JWT Authentication** - Secure token-based auth
- **Form Validation** - Client-side validation with custom rules
- **Hash Router** - SPA navigation without server config
- **DataTable Component** - Sortable, searchable tables
- **Toast Notifications** - User-friendly alerts
- **Docker Ready** - Multi-stage Dockerfile with nginx

## Tech Stack

| Category | Technology |
|----------|------------|
| Core | jQuery 3.7.1 |
| UI Framework | Bootstrap 5.3 |
| Bundler | Parcel 2.10 |
| HTTP Client | Axios 1.6 |
| Notifications | Toastify.js |
| Containerization | Docker + nginx |

## Project Structure

```
├── src/
│   ├── js/
│   │   ├── main.js                 # Entry point
│   │   ├── config.js               # Configuration
│   │   ├── api.js                  # HTTP client
│   │   ├── utils.js                # Utilities
│   │   ├── modules/
│   │   │   ├── auth.js             # Authentication
│   │   │   ├── users.js            # User management
│   │   │   ├── router.js           # SPA router
│   │   │   └── notifications.js    # Notifications
│   │   ├── components/
│   │   │   ├── header.js           # Header component
│   │   │   ├── sidebar.js          # Sidebar component
│   │   │   ├── modal.js            # Modal component
│   │   │   ├── datatable.js        # DataTable component
│   │   │   └── form-validator.js   # Form validator
│   │   └── pages/
│   │       ├── home.js             # Home page
│   │       ├── login.js            # Login page
│   │       └── dashboard.js        # Dashboard page
│   ├── css/
│   │   ├── main.css                # Main styles
│   │   ├── components.css          # Component styles
│   │   └── utilities.css           # Utility classes
│   ├── templates/
│   │   ├── header.html             # Header template
│   │   ├── sidebar.html            # Sidebar template
│   │   └── login-form.html         # Login form template
│   └── index.html                  # Entry HTML
├── package.json
├── .env.example
├── Dockerfile
├── nginx.conf
└── README.md
```

## Quick Start

### Prerequisites

- Node.js 20+
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Edit .env with your settings
nano .env
```

### Development

```bash
# Start dev server with hot reload
npm run dev

# Application will be available at http://localhost:1234
```

### Production Build

```bash
# Build for production
npm run build

# Output will be in dist/ folder
```

### Docker Deployment

```bash
# Build Docker image
docker build -t {{PROJECT_NAME}}:latest .

# Run container
docker run -p 80:80 {{PROJECT_NAME}}:latest
```

## Configuration

Create a `.env` file based on `.env.example`:

```env
API_URL=http://localhost:8000
API_TIMEOUT=30000
APP_NAME={{PROJECT_NAME}}
APP_ENV=development
ENABLE_ANALYTICS=false
ENABLE_DEBUG=true
TOKEN_STORAGE_KEY=auth_token
REFRESH_TOKEN_KEY=refresh_token
```

## Architecture

### Module Pattern

All JavaScript follows a modular pattern with clear separation of concerns:

```javascript
// Singleton module
class MyModule {
    constructor() {
        this.state = {};
    }

    init() {
        // Initialize module
    }
}

export const myModule = new MyModule();
```

### Component Pattern

Components encapsulate UI logic and rendering:

```javascript
export class MyComponent {
    constructor(container, options = {}) {
        this.container = container;
        this.options = options;
    }

    render() {
        // Render component
    }

    attachEventListeners() {
        // Attach events
    }

    destroy() {
        // Cleanup
    }
}
```

### Router

Simple hash-based routing:

```javascript
// Register route
router.register('dashboard', async () => {
    const { DashboardPage } = await import('./pages/dashboard.js');
    const page = new DashboardPage();
    await page.render();
}, { requiresAuth: true, title: 'Dashboard' });

// Navigate
router.navigate('dashboard');
```

### API Client

Axios-based HTTP client with interceptors:

```javascript
// GET request
const data = await api.get('/users');

// POST request
const user = await api.post('/users', userData);

// Automatic token refresh on 401
// Error handling with notifications
```

### Form Validation

Declarative validation rules:

```javascript
import { FormValidator, Rules } from './components/form-validator.js';

const validator = new FormValidator('#my-form', {
    email: [
        Rules.required('Email is required'),
        Rules.email()
    ],
    password: [
        Rules.required('Password is required'),
        Rules.minLength(8)
    ]
});

if (validator.validate()) {
    // Form is valid
}
```

### DataTable

Sortable, searchable, paginated tables:

```javascript
const table = new DataTable('#table-container', {
    columns: [
        { field: 'id', label: 'ID' },
        { field: 'name', label: 'Name' },
        { field: 'created_at', label: 'Date', type: 'datetime' }
    ],
    data: users,
    perPage: 10,
    actions: (row) => `
        <button class="btn btn-sm btn-primary" data-id="${row.id}">
            Edit
        </button>
    `
});

table.render();
```

## Scripts

```bash
# Development
npm run dev              # Start dev server

# Production
npm run build            # Build for production
npm run clean            # Clean build files

# Code Quality
npm run lint             # Lint JavaScript
npm run format           # Format code with Prettier
```

## Browser Support

- Chrome (last 2 versions)
- Firefox (last 2 versions)
- Safari (last 2 versions)
- Edge (last 2 versions)
- No IE11 support

## Best Practices

### Code Organization

- **One component per file** - Max 200-300 lines
- **Clear naming** - Descriptive function and variable names
- **ES6+ features** - Use modern JavaScript
- **Async/await** - For asynchronous operations

### Security

- **XSS Protection** - Escape user input with `escapeHtml()`
- **CSRF Protection** - Use tokens for state-changing requests
- **Secure storage** - Use httpOnly cookies for sensitive data
- **Content Security Policy** - Configure CSP headers in nginx

### Performance

- **Lazy loading** - Dynamic imports for pages
- **Debouncing** - Use `debounce()` for search/input handlers
- **Caching** - Cache API responses where appropriate
- **Code splitting** - Parcel handles automatically

### Accessibility

- **Semantic HTML** - Use proper HTML5 elements
- **ARIA labels** - Add where needed
- **Keyboard navigation** - Test with keyboard only
- **Screen reader** - Test with screen readers

## Deployment

### Static Hosting

Build and deploy to any static host:

```bash
npm run build
# Upload dist/ folder to your host
```

### Docker

Use the provided Dockerfile:

```bash
docker build -t my-app .
docker run -p 80:80 my-app
```

### nginx Configuration

The included `nginx.conf` provides:

- SPA routing (fallback to index.html)
- Gzip compression
- Security headers
- Static asset caching
- Health check endpoint

## Troubleshooting

### Dev server won't start

```bash
# Clear cache
npm run clean
rm -rf .parcel-cache

# Reinstall dependencies
rm -rf node_modules
npm install
```

### Build fails

```bash
# Check Node version
node --version  # Should be 20+

# Update dependencies
npm update
```

### API requests fail

- Check API_URL in .env
- Verify API is running
- Check browser console for CORS errors
- Verify authentication token

## Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing`)
5. Open Pull Request

## License

MIT

## Author

{{AUTHOR}}

---

## Changelog

### [1.0.0] - {{DATE}}

#### Added
- Initial project setup with jQuery 3.7
- Bootstrap 5 UI framework integration
- Modular architecture with ES6+ modules
- JWT authentication system
- Hash-based SPA router
- Form validation component
- DataTable component with sort/search/pagination
- Toast notifications with Toastify
- Multi-stage Docker build with nginx
- Complete documentation

#### Features
- User authentication (login/logout/token refresh)
- Dashboard with stats cards
- Responsive sidebar navigation
- Modal component for dialogs
- Utility functions library
- API client with interceptors
- Development and production configs

---

Built with modern web standards and best practices.
