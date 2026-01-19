# Developer Experience (DevEx)

> Tools and configurations for enhanced developer productivity
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for optimizing the developer experience:

- **DevContainers**: VS Code remote containers
- **Storybook**: Component documentation and development
- **MSW**: API mocking for development and testing
- **Code Quality**: ESLint, Prettier, Husky, lint-staged
- **Database Seeding**: Test data generators
- **Documentation**: Auto-generated docs

---

## Directory Structure

```
devex/
├── devcontainer/                # DevContainer configurations
│   ├── devcontainer.json.tmpl
│   ├── Dockerfile.tmpl
│   └── docker-compose.yml.tmpl
│
├── storybook/                   # Storybook setup
│   ├── main.ts.tmpl
│   ├── preview.ts.tmpl
│   └── stories/
│       ├── Button.stories.tsx.tmpl
│       └── Introduction.stories.mdx.tmpl
│
├── msw/                         # Mock Service Worker
│   ├── browser.ts.tmpl          # Browser setup
│   ├── server.ts.tmpl           # Node.js setup
│   └── handlers/
│       ├── auth.ts.tmpl
│       └── users.ts.tmpl
│
├── quality/                     # Code quality tools
│   ├── .eslintrc.json.tmpl
│   ├── .prettierrc.json.tmpl
│   ├── .husky/
│   │   ├── pre-commit.tmpl
│   │   └── commit-msg.tmpl
│   └── lint-staged.config.js.tmpl
│
├── seed/                        # Database seeding
│   ├── seed.ts.tmpl             # TypeScript
│   ├── seed.py.tmpl             # Python
│   └── factories/
│       ├── user.factory.ts.tmpl
│       └── post.factory.ts.tmpl
│
└── docs/                        # Documentation
    ├── typedoc.json.tmpl
    ├── sphinx/                  # Python docs
    │   └── conf.py.tmpl
    └── templates/
        └── README.template.md
```

---

## DevContainers

### Why DevContainers?

- **Consistency**: Same environment for all developers
- **Onboarding**: Setup in minutes, not hours
- **Isolation**: No conflicts with local machine
- **Integration**: VS Code extensions pre-installed

### Setup

```json
// .devcontainer/devcontainer.json
{
  "name": "{{PROJECT_NAME}}",
  "dockerComposeFile": "docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspace",

  "features": {
    "ghcr.io/devcontainers/features/node:1": {
      "version": "20"
    },
    "ghcr.io/devcontainers/features/python:1": {
      "version": "3.12"
    }
  },

  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "bradlc.vscode-tailwindcss",
        "ms-python.python",
        "ms-azuretools.vscode-docker"
      ],
      "settings": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      }
    }
  },

  "postCreateCommand": "npm install && npm run setup",
  "forwardPorts": [3000, 8000, 5432]
}
```

### Docker Compose

```yaml
# .devcontainer/docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ../..:/workspace:cached
    command: sleep infinity
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/dev

  db:
    image: postgres:16-alpine
    restart: unless-stopped
    volumes:
      - postgres-data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: dev

  redis:
    image: redis:7-alpine
    restart: unless-stopped

volumes:
  postgres-data:
```

### Usage

```bash
# Open in VS Code
code .

# Command Palette (Cmd+Shift+P)
> Dev Containers: Reopen in Container

# Everything is configured and ready!
```

---

## Storybook

### Why Storybook?

- **Component Documentation**: Visual component library
- **Isolated Development**: Build components in isolation
- **Testing**: Visual regression testing
- **Design System**: Centralized UI patterns

### Setup

```bash
# Install
npx storybook@latest init

# Dependencies already configured
```

### Configuration

```typescript
// .storybook/main.ts
import type { StorybookConfig } from '@storybook/react-vite';

const config: StorybookConfig = {
  stories: ['../src/**/*.mdx', '../src/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-interactions',
    '@storybook/addon-a11y',
  ],
  framework: {
    name: '@storybook/react-vite',
    options: {},
  },
};

export default config;
```

### Example Story

```typescript
// src/components/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './Button';

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  parameters: {
    layout: 'centered',
  },
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'danger'],
    },
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: {
    variant: 'primary',
    children: 'Primary Button',
  },
};

export const Secondary: Story = {
  args: {
    variant: 'secondary',
    children: 'Secondary Button',
  },
};

export const Disabled: Story = {
  args: {
    variant: 'primary',
    children: 'Disabled',
    disabled: true,
  },
};
```

### Run Storybook

```bash
# Development
npm run storybook

# Build static
npm run build-storybook

# Opens at http://localhost:6006
```

---

## Mock Service Worker (MSW)

### Why MSW?

- **API Mocking**: Mock REST/GraphQL APIs
- **Development**: Work without backend
- **Testing**: Deterministic tests
- **Offline**: Continue development offline

### Setup

```bash
# Install
npm install msw --save-dev

# Initialize
npx msw init public/ --save
```

### Handlers

```typescript
// msw/handlers/auth.ts
import { http, HttpResponse } from 'msw';

export const authHandlers = [
  // Login
  http.post('/api/auth/login', async ({ request }) => {
    const { email, password } = await request.json();

    if (email === 'test@example.com' && password === 'password') {
      return HttpResponse.json({
        token: 'fake-jwt-token',
        user: {
          id: '1',
          email: 'test@example.com',
          name: 'Test User',
        },
      });
    }

    return HttpResponse.json(
      { error: 'Invalid credentials' },
      { status: 401 }
    );
  }),

  // Register
  http.post('/api/auth/register', async ({ request }) => {
    const body = await request.json();
    return HttpResponse.json({
      user: { id: '2', ...body },
    }, { status: 201 });
  }),
];
```

### Browser Setup

```typescript
// msw/browser.ts
import { setupWorker } from 'msw/browser';
import { authHandlers } from './handlers/auth';
import { userHandlers } from './handlers/users';

export const worker = setupWorker(...authHandlers, ...userHandlers);

// main.tsx
if (import.meta.env.DEV) {
  const { worker } = await import('./msw/browser');
  worker.start({
    onUnhandledRequest: 'bypass',
  });
}
```

### Node.js Setup (Tests)

```typescript
// msw/server.ts
import { setupServer } from 'msw/node';
import { authHandlers } from './handlers/auth';

export const server = setupServer(...authHandlers);

// tests/setup.ts
import { beforeAll, afterEach, afterAll } from 'vitest';
import { server } from '../msw/server';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

---

## Code Quality

### ESLint

```json
// .eslintrc.json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "react", "react-hooks"],
  "rules": {
    "react/react-in-jsx-scope": "off",
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_" }]
  }
}
```

### Prettier

```json
// .prettierrc.json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

### Husky + lint-staged

```bash
# Install
npm install --save-dev husky lint-staged

# Initialize
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npx lint-staged"
```

```javascript
// lint-staged.config.js
module.exports = {
  '*.{js,jsx,ts,tsx}': ['eslint --fix', 'prettier --write'],
  '*.{json,md,yml}': ['prettier --write'],
};
```

### Commitlint

```bash
# Install
npm install --save-dev @commitlint/cli @commitlint/config-conventional

# Configure
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# Add hook
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'
```

---

## Database Seeding

### TypeScript (Prisma)

```typescript
// seed/seed.ts
import { PrismaClient } from '@prisma/client';
import { UserFactory } from './factories/user.factory';

const prisma = new PrismaClient();

async function main() {
  console.log('🌱 Seeding database...');

  // Clear existing data
  await prisma.user.deleteMany();

  // Create users
  const users = await Promise.all(
    Array.from({ length: 10 }).map(() =>
      UserFactory.create(prisma)
    )
  );

  console.log(`✅ Created ${users.length} users`);
}

main()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

### Factory Pattern

```typescript
// seed/factories/user.factory.ts
import { faker } from '@faker-js/faker';
import { PrismaClient } from '@prisma/client';

export class UserFactory {
  static create(prisma: PrismaClient) {
    return prisma.user.create({
      data: {
        email: faker.internet.email(),
        name: faker.person.fullName(),
        password: faker.internet.password(),
        avatar: faker.image.avatar(),
      },
    });
  }

  static createMany(prisma: PrismaClient, count: number) {
    return Promise.all(
      Array.from({ length: count }).map(() => this.create(prisma))
    );
  }
}
```

### Python (SQLModel)

```python
# seed/seed.py
from sqlmodel import Session, create_engine
from faker import Faker
from models import User

fake = Faker()
engine = create_engine("postgresql://...")

def seed_users(session: Session, count: int = 10):
    users = [
        User(
            email=fake.email(),
            name=fake.name(),
            password=fake.password()
        )
        for _ in range(count)
    ]
    session.add_all(users)
    session.commit()
    return users

if __name__ == "__main__":
    with Session(engine) as session:
        users = seed_users(session)
        print(f"✅ Created {len(users)} users")
```

### Run Seeding

```bash
# TypeScript
npm run seed

# Python
python seed/seed.py
```

---

## Documentation

### TypeDoc (TypeScript)

```json
// typedoc.json
{
  "entryPoints": ["src/index.ts"],
  "out": "docs",
  "plugin": ["typedoc-plugin-markdown"],
  "excludePrivate": true,
  "excludeInternal": true
}
```

```bash
# Generate docs
npx typedoc

# Output: docs/
```

### Sphinx (Python)

```python
# docs/conf.py
project = '{{PROJECT_NAME}}'
author = '{{AUTHOR}}'
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',
]
```

```bash
# Generate docs
cd docs
sphinx-build -b html . _build

# Output: docs/_build/
```

---

## Scripts

### package.json

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,json,md}\"",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build",
    "seed": "tsx seed/seed.ts",
    "docs": "typedoc"
  }
}
```

---

## VS Code Settings

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter"
  }
}
```

### Recommended Extensions

```json
// .vscode/extensions.json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "ms-python.python",
    "ms-azuretools.vscode-docker",
    "prisma.prisma",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

---

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/quality.yml
name: Code Quality

on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Type check
        run: npm run type-check

      - name: Lint
        run: npm run lint

      - name: Format check
        run: npm run format -- --check

      - name: Test
        run: npm test
```

---

## Best Practices

### DevContainer

- Keep it minimal
- Document custom commands
- Use features instead of manual installs

### Storybook

- Document component props
- Include accessibility checks
- Add interaction tests

### MSW

- Keep handlers realistic
- Test error scenarios
- Don't mock everything

### Code Quality

- Run checks in CI
- Fix warnings, not just errors
- Keep config files simple

---

## Resources

### Documentation

- [DevContainers](https://containers.dev/)
- [Storybook](https://storybook.js.org/)
- [MSW](https://mswjs.io/)
- [ESLint](https://eslint.org/)
- [Prettier](https://prettier.io/)
- [Husky](https://typicode.github.io/husky/)

---

## Support

For issues or questions:
1. Check the tool documentation
2. Review examples
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
