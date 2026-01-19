# Phase 3: Implementation Guide for Wizard Developers

**Audience**: Developers implementing the enhanced Phase 3 wizard
**Purpose**: Practical guide for implementation, validation, and code generation

---

## 1. Implementation Checklist

### Phase 1: Core Questions (Days 1-3)

- [ ] **Q1: Architecture Pattern** (CRITICAL FIRST)
  - [ ] Implement conditional logic (affects all downstream choices)
  - [ ] Add detailed descriptions + pros/cons
  - [ ] Add recommendation badges (★ for Modular Monolith)

- [ ] **Q2: Backend Framework**
  - [ ] Add Go + Fiber option
  - [ ] Update descriptions with performance metrics
  - [ ] Add conditional recommendations based on architecture

- [ ] **Q3-Q4: Frontend (Multi-platform)**
  - [ ] Split into "platforms" (multi-select) + "framework" (conditional)
  - [ ] Add React Native + Expo templates
  - [ ] Add Tauri templates (desktop)
  - [ ] Add Remix and Astro as Next.js alternatives

### Phase 2: API & Data Layer (Days 4-6)

- [ ] **Q5: API Style**
  - [ ] Multi-select (REST + GraphQL + gRPC can coexist)
  - [ ] Conditional recommendation (gRPC if microservices)
  - [ ] Add code generation for each style

- [ ] **Q6-9: Databases**
  - [ ] Add MongoDB as primary database option
  - [ ] Add Redis caching layer (recommend by default)
  - [ ] Add vector database section (pgvector/Qdrant/Pinecone)
  - [ ] Add search engine section (Elasticsearch/Typesense)

### Phase 3: Deployment (Days 7-8)

- [ ] **Q10: Deployment Strategy**
  - [ ] Add 4 options (traditional/serverless/edge/hybrid)
  - [ ] Generate appropriate configs (Dockerfile, vercel.json, K8s manifests)

### Phase 4: Validation & Recommendations (Days 9-10)

- [ ] Implement validation rules (see YAML)
- [ ] Build recommendation engine
- [ ] Test all combinations

---

## 2. Conditional Logic Map

### Architecture → Backend Recommendations

```typescript
if (architecture === 'microservices') {
  recommend('go_fiber');
  recommend('grpc');
} else if (architecture === 'event_driven') {
  recommend('go_fiber');
  note('Will need message broker (Kafka/RabbitMQ)');
} else {
  recommend('fastapi'); // Rapid development
}
```

### Backend → Database Recommendations

```typescript
if (backend === 'none') {
  require(['supabase', 'firebase']);
} else if (backend === 'go_fiber') {
  recommend('postgresql'); // Best Go support
  recommend('redis');
}
```

### Frontend Platforms → Framework Options

```typescript
if (frontendPlatforms.includes('web')) {
  askWebFramework(); // Next.js/Remix/Astro
}

if (frontendPlatforms.includes('mobile')) {
  scaffoldReactNative();
}

if (frontendPlatforms.includes('desktop')) {
  scaffoldTauri();
}
```

### Database → Vector Database

```typescript
if (primaryDatabase === 'postgresql' || primaryDatabase === 'supabase') {
  recommend('pgvector'); // Same DB, easier
} else {
  recommend('qdrant'); // Separate service
}
```

### Architecture → Deployment

```typescript
if (architecture === 'monolith') {
  recommend('serverless');
} else if (architecture === 'microservices') {
  recommend('traditional'); // K8s
} else if (architecture === 'event_driven') {
  recommend('traditional'); // Need stateful message brokers
}
```

---

## 3. Code Generation Strategy

### Template Structure

```
templates/
├── backend/
│   ├── fastapi/
│   │   ├── base/              # Core FastAPI setup
│   │   ├── with-graphql/      # + Strawberry GraphQL
│   │   ├── with-grpc/         # + gRPC
│   │   └── with-ai/           # + pgvector/embeddings
│   ├── nestjs/
│   │   ├── base/
│   │   ├── with-graphql/
│   │   └── with-grpc/
│   └── go/
│       ├── fiber-rest/
│       ├── fiber-grpc/
│       └── microservices/     # Multiple services
│
├── frontend/
│   ├── nextjs/
│   │   ├── app-router/        # Next.js 15
│   │   ├── with-apollo/       # + GraphQL
│   │   └── edge-runtime/      # Edge deployment
│   ├── remix/
│   ├── astro/
│   ├── react-native/
│   │   └── expo/
│   └── tauri/
│
├── database/
│   ├── supabase/              # Supabase config + migrations
│   ├── postgresql/            # Docker + migrations
│   ├── mongodb/               # Docker + indexes
│   └── redis/                 # Docker + basic config
│
├── ai/
│   ├── pgvector/              # PostgreSQL extension setup
│   ├── qdrant/                # Docker + config
│   ├── pinecone/              # SDK setup
│   └── embeddings/            # OpenAI/Cohere integration
│
├── search/
│   ├── elasticsearch/         # Docker + mapping
│   └── typesense/             # Docker + schema
│
└── deployment/
    ├── traditional/
    │   ├── docker/            # Dockerfile, docker-compose
    │   └── k8s/               # Kubernetes manifests
    ├── serverless/
    │   ├── vercel/            # vercel.json
    │   ├── aws-lambda/        # SAM template
    │   └── cloud-run/         # Cloud Run config
    └── edge/
        └── vercel-edge/       # Edge config
```

### Template Composition Logic

```typescript
function generateProject(config: WizardConfig) {
  const templates: string[] = [];

  // 1. Base backend
  templates.push(`backend/${config.backend}/base`);

  // 2. Add API styles
  if (config.apiStyle.includes('graphql')) {
    templates.push(`backend/${config.backend}/with-graphql`);
  }
  if (config.apiStyle.includes('grpc')) {
    templates.push(`backend/${config.backend}/with-grpc`);
  }

  // 3. Frontend platforms
  if (config.frontendPlatforms.includes('web')) {
    templates.push(`frontend/${config.webFramework}`);

    if (config.apiStyle.includes('graphql')) {
      templates.push(`frontend/${config.webFramework}/with-apollo`);
    }

    if (config.deploymentStrategy === 'edge') {
      templates.push(`frontend/${config.webFramework}/edge-runtime`);
    }
  }

  if (config.frontendPlatforms.includes('mobile')) {
    templates.push('frontend/react-native/expo');
  }

  if (config.frontendPlatforms.includes('desktop')) {
    templates.push('frontend/tauri');
  }

  // 4. Database
  templates.push(`database/${config.primaryDatabase}`);

  if (config.caching === 'redis') {
    templates.push('database/redis');
  }

  if (config.vectorDatabase !== 'none') {
    templates.push(`ai/${config.vectorDatabase}`);
    templates.push('ai/embeddings'); // OpenAI/Cohere client
  }

  if (config.searchEngine !== 'none') {
    templates.push(`search/${config.searchEngine}`);
  }

  // 5. Deployment
  templates.push(`deployment/${config.deploymentStrategy}`);

  // 6. Merge templates
  return mergeTemplates(templates, config);
}
```

---

## 4. Template Variables

### Backend Template Variables

```typescript
interface BackendTemplateVars {
  projectName: string;
  backend: 'fastapi' | 'nestjs' | 'go_fiber' | 'none';
  apiStyles: ('rest' | 'graphql' | 'grpc')[];
  database: {
    type: 'supabase' | 'postgresql' | 'mongodb' | 'firebase';
    url: string;
  };
  cache?: {
    type: 'redis' | 'in_memory';
    url?: string;
  };
  vector?: {
    type: 'pgvector' | 'qdrant' | 'pinecone';
    dimension: number; // 1536 for OpenAI, 768 for Cohere
  };
  search?: {
    type: 'elasticsearch' | 'typesense';
    url: string;
  };
}
```

### Frontend Template Variables

```typescript
interface FrontendTemplateVars {
  projectName: string;
  framework: 'nextjs' | 'remix' | 'astro';
  platforms: ('web' | 'mobile' | 'desktop')[];
  apiUrl: string;
  apiStyle: 'rest' | 'graphql';
  auth?: {
    provider: 'supabase' | 'firebase' | 'jwt';
  };
}
```

### Example Template File

```python
# apps/api/src/api/config.py.template
"""
config.py - Configuration

Autor: Homero Thompson del Lago del Terror
"""

from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str = "{{database.url}}"
    {% if cache %}
    redis_url: str = "{{cache.url}}"
    {% endif %}
    {% if vector %}
    vector_db_url: str = "{{vector.url}}"
    {% endif %}
    {% if search %}
    search_url: str = "{{search.url}}"
    {% endif %}

    class Config:
        env_file = ".env"

settings = Settings()
```

---

## 5. Validation Rules Implementation

```typescript
// Validation rule engine
type ValidationRule = {
  condition: (config: WizardConfig) => boolean;
  action: 'error' | 'warn' | 'recommend' | 'note';
  message: string;
};

const validationRules: ValidationRule[] = [
  // Conflict: Go + NestJS
  {
    condition: (config) =>
      config.backend === 'go_fiber' && config.backend === 'nestjs',
    action: 'error',
    message: 'Choose either Go or NestJS, not both.',
  },

  // Requirement: API-less needs BaaS
  {
    condition: (config) =>
      config.backend === 'none' &&
      !['supabase', 'firebase'].includes(config.primaryDatabase),
    action: 'error',
    message: 'API-less requires Supabase or Firebase.',
  },

  // Recommendation: Microservices + gRPC
  {
    condition: (config) =>
      config.architecturePattern === 'microservices' &&
      !config.apiStyle.includes('grpc'),
    action: 'recommend',
    message: 'Consider adding gRPC for inter-service communication.',
  },

  // Recommendation: pgvector requires PostgreSQL
  {
    condition: (config) =>
      config.vectorDatabase === 'pgvector' &&
      !['postgresql', 'supabase'].includes(config.primaryDatabase),
    action: 'error',
    message: 'pgvector requires PostgreSQL or Supabase.',
  },

  // Note: Event-driven needs message broker
  {
    condition: (config) => config.architecturePattern === 'event_driven',
    action: 'note',
    message:
      'Event-driven architecture will require message broker (Kafka/RabbitMQ) in infrastructure phase.',
  },

  // Warning: Edge + stateful backend
  {
    condition: (config) =>
      config.deploymentStrategy === 'edge' && config.backend !== 'none',
    action: 'warn',
    message:
      'Edge deployment works best with API-less or serverless backends.',
  },
];

function validateConfig(config: WizardConfig): ValidationResult {
  const errors: string[] = [];
  const warnings: string[] = [];
  const recommendations: string[] = [];
  const notes: string[] = [];

  for (const rule of validationRules) {
    if (rule.condition(config)) {
      switch (rule.action) {
        case 'error':
          errors.push(rule.message);
          break;
        case 'warn':
          warnings.push(rule.message);
          break;
        case 'recommend':
          recommendations.push(rule.message);
          break;
        case 'note':
          notes.push(rule.message);
          break;
      }
    }
  }

  return { errors, warnings, recommendations, notes };
}
```

---

## 6. Recommendation Engine

```typescript
type StackPreset = {
  name: string;
  description: string;
  tags: string[];
  config: Partial<WizardConfig>;
};

const presets: StackPreset[] = [
  {
    name: 'Modern Startup Stack',
    description: 'Fast development, scales to Series B, AI-ready',
    tags: ['startup', 'mvp', 'ai', 'rapid-dev'],
    config: {
      architecturePattern: 'modular_monolith',
      backend: 'fastapi',
      frontendPlatforms: ['web', 'mobile'],
      webFramework: 'nextjs',
      apiStyle: ['rest'],
      primaryDatabase: 'supabase',
      caching: 'redis',
      vectorDatabase: 'pgvector',
      deploymentStrategy: 'serverless',
    },
  },

  {
    name: 'High-Performance Microservices',
    description: 'Maximum performance, proven at scale',
    tags: ['enterprise', 'microservices', 'performance', 'scalability'],
    config: {
      architecturePattern: 'microservices',
      backend: 'go_fiber',
      apiStyle: ['rest', 'grpc'],
      primaryDatabase: 'postgresql',
      caching: 'redis',
      deploymentStrategy: 'traditional',
    },
  },

  {
    name: 'AI-First Application',
    description: 'Python ecosystem, vector search, RAG',
    tags: ['ai', 'ml', 'embeddings', 'rag'],
    config: {
      architecturePattern: 'modular_monolith',
      backend: 'fastapi',
      webFramework: 'nextjs',
      apiStyle: ['rest', 'graphql'],
      primaryDatabase: 'postgresql',
      caching: 'redis',
      vectorDatabase: 'pgvector',
      searchEngine: 'elasticsearch',
      deploymentStrategy: 'serverless',
    },
  },

  {
    name: 'Global Edge-First App',
    description: '<50ms latency globally, edge computing',
    tags: ['edge', 'global', 'low-latency', 'serverless'],
    config: {
      architecturePattern: 'monolith',
      backend: 'none',
      frontendPlatforms: ['web'],
      webFramework: 'nextjs',
      primaryDatabase: 'supabase',
      caching: 'redis',
      deploymentStrategy: 'edge',
    },
  },

  {
    name: 'Event-Driven E-commerce',
    description: 'Real-time, scalable, event sourcing',
    tags: ['event-driven', 'ecommerce', 'real-time', 'scalable'],
    config: {
      architecturePattern: 'event_driven',
      backend: 'go_fiber',
      apiStyle: ['rest', 'grpc'],
      primaryDatabase: 'postgresql',
      caching: 'redis',
      searchEngine: 'elasticsearch',
      deploymentStrategy: 'traditional',
    },
  },
];

function recommendPreset(userAnswers: Partial<WizardConfig>): StackPreset | null {
  // Score each preset based on user answers
  const scores = presets.map((preset) => {
    let score = 0;

    // Architecture match
    if (preset.config.architecturePattern === userAnswers.architecturePattern) {
      score += 10;
    }

    // Backend match
    if (preset.config.backend === userAnswers.backend) {
      score += 5;
    }

    // Deployment match
    if (preset.config.deploymentStrategy === userAnswers.deploymentStrategy) {
      score += 5;
    }

    // Tag matching (from earlier questions)
    if (userAnswers.tags) {
      const matchingTags = preset.tags.filter((tag) =>
        userAnswers.tags.includes(tag)
      );
      score += matchingTags.length * 2;
    }

    return { preset, score };
  });

  // Return highest scoring preset
  const best = scores.sort((a, b) => b.score - a.score)[0];
  return best.score > 10 ? best.preset : null;
}
```

---

## 7. Testing Strategy

### Unit Tests

```typescript
describe('Phase 3 Wizard', () => {
  describe('Validation', () => {
    it('should error if API-less without Supabase/Firebase', () => {
      const config = {
        backend: 'none',
        primaryDatabase: 'postgresql',
      };
      const result = validateConfig(config);
      expect(result.errors).toContain('API-less requires Supabase or Firebase');
    });

    it('should recommend gRPC for microservices', () => {
      const config = {
        architecturePattern: 'microservices',
        apiStyle: ['rest'],
      };
      const result = validateConfig(config);
      expect(result.recommendations).toContain(
        'Consider adding gRPC for inter-service communication'
      );
    });
  });

  describe('Template Generation', () => {
    it('should generate FastAPI + GraphQL + pgvector', async () => {
      const config = {
        backend: 'fastapi',
        apiStyle: ['rest', 'graphql'],
        vectorDatabase: 'pgvector',
      };
      const files = await generateProject(config);

      expect(files).toContain('apps/api/src/api/graphql/schema.py');
      expect(files).toContain('apps/api/src/api/shared/vector.py');
    });

    it('should generate Go microservices with gRPC', async () => {
      const config = {
        architecturePattern: 'microservices',
        backend: 'go_fiber',
        apiStyle: ['rest', 'grpc'],
      };
      const files = await generateProject(config);

      expect(files).toContain('services/gateway/cmd/main.go');
      expect(files).toContain('proto/auth.proto');
      expect(files).toContain('k8s/gateway/deployment.yaml');
    });
  });

  describe('Recommendations', () => {
    it('should recommend Modern Startup Stack for MVP tag', () => {
      const answers = {
        tags: ['mvp', 'ai'],
        architecturePattern: 'modular_monolith',
      };
      const preset = recommendPreset(answers);
      expect(preset?.name).toBe('Modern Startup Stack');
    });
  });
});
```

### Integration Tests

```typescript
describe('End-to-End Wizard Flow', () => {
  it('should complete full wizard and generate valid project', async () => {
    const wizard = new Phase3Wizard();

    // Answer all questions
    wizard.answer('architecturePattern', 'modular_monolith');
    wizard.answer('backend', 'fastapi');
    wizard.answer('frontendPlatforms', ['web', 'mobile']);
    wizard.answer('webFramework', 'nextjs');
    wizard.answer('apiStyle', ['rest', 'graphql']);
    wizard.answer('primaryDatabase', 'supabase');
    wizard.answer('caching', 'redis');
    wizard.answer('vectorDatabase', 'pgvector');
    wizard.answer('searchEngine', 'none');
    wizard.answer('deploymentStrategy', 'serverless');

    // Generate project
    const project = await wizard.generate();

    // Validate structure
    expect(project.files).toHaveLength(50); // Approx
    expect(project.files).toContain('apps/api/src/api/main.py');
    expect(project.files).toContain('apps/web/app/layout.tsx');
    expect(project.files).toContain('apps/mobile/app.json');

    // Validate can run
    const result = await runCommand('npm install', project.dir);
    expect(result.exitCode).toBe(0);
  });
});
```

---

## 8. UI/UX Considerations

### Question Flow

```
┌─────────────────────────────────────┐
│  Phase 3: Stack Selection           │
├─────────────────────────────────────┤
│                                     │
│  Progress: ▓▓▓▓▓▓░░░░ 6/10         │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Q6: Primary Database        │   │
│  │                             │   │
│  │ Choose your main database:  │   │
│  │                             │   │
│  │ ○ Supabase                  │   │
│  │   PostgreSQL + Auth + ...   │   │
│  │   ★ RECOMMENDED             │   │
│  │                             │   │
│  │ ○ PostgreSQL (self)         │   │
│  │   Full control              │   │
│  │                             │   │
│  │ ○ MongoDB                   │   │
│  │   Flexible schema           │   │
│  │   Best for: Catalogs, CMS   │   │
│  │                             │   │
│  │ ○ Firebase                  │   │
│  │   Real-time                 │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Back]              [Next]         │
└─────────────────────────────────────┘
```

### Conditional Questions

```typescript
// Skip Q4 (Web Framework) if not web
if (!frontendPlatforms.includes('web')) {
  skipQuestion('webFramework');
}

// Show note after Q1
if (architecturePattern === 'event_driven') {
  showNote('Will require message broker in infrastructure phase');
}

// Show recommendation
if (architecturePattern === 'microservices' && backend !== 'go_fiber') {
  showRecommendation('Consider Go + Fiber for best performance');
}
```

### Summary Screen

```
┌─────────────────────────────────────────────┐
│  Stack Summary                              │
├─────────────────────────────────────────────┤
│                                             │
│  Architecture:  Modular Monolith            │
│  Backend:       FastAPI (Python)            │
│  Frontend:      Next.js + React Native      │
│  API:           REST + GraphQL              │
│  Database:      Supabase (PostgreSQL)       │
│  Caching:       Redis                       │
│  Vector DB:     pgvector                    │
│  Deployment:    Serverless (Vercel)         │
│                                             │
│  ✓ 47 files will be generated               │
│  ✓ No validation errors                     │
│  ⚠ 1 recommendation:                        │
│    • Consider Elasticsearch for search      │
│                                             │
│  Estimated setup time: 10 minutes           │
│  Estimated monthly cost: $50-100            │
│                                             │
│  [Edit]  [Generate Project]                 │
└─────────────────────────────────────────────┘
```

---

## 9. Post-Generation Steps

### After project generation:

1. **README.md** with:
   - Stack explanation
   - Quick start guide
   - Development setup
   - Deployment instructions

2. **ARCHITECTURE.md** with:
   - System diagram
   - Technology choices explanation
   - Scalability considerations
   - Migration paths

3. **CONTRIBUTING.md** with:
   - Development workflow
   - Testing strategy
   - Code style guide

4. **GitHub Actions** workflows:
   - Tests
   - Linting
   - Build
   - Deploy (preview + production)

---

## 10. Future Enhancements

### Phase 4 Ideas

1. **Cost Estimation**
   - Show estimated monthly costs per stack
   - Breakdown: compute, storage, services

2. **Smart Defaults**
   - Learn from user's previous projects
   - Industry-based recommendations

3. **Migration Guides**
   - Monolith → Microservices
   - REST → GraphQL
   - PostgreSQL → MongoDB (or vice versa)

4. **Interactive Preview**
   - Show file tree before generation
   - Preview key files

5. **Community Templates**
   - Share custom stacks
   - Star/fork popular configurations

---

## Summary

**Implementation Priority**:
1. Core questions (Q1-Q6) - Week 1
2. Advanced options (Q7-Q10) - Week 2
3. Code generation - Week 3
4. Testing & validation - Week 4

**Success Metrics**:
- ✅ 90% use case coverage
- ✅ <10 questions (currently 10)
- ✅ <5 minutes to complete
- ✅ Generated projects run without errors

**Files Delivered**:
- `/home/user/projects/phase3-stack-enhanced.yaml` - Complete YAML
- `/home/user/projects/phase3-analysis.md` - Gap analysis
- `/home/user/projects/phase3-comparison.md` - Before/after comparison
- `/home/user/projects/phase3-code-examples.md` - Code generation examples
- `/home/user/projects/phase3-implementation-guide.md` - This guide

---

**Author**: Senior Software Architect
**Date**: 2026-01-19
**Status**: Ready for Implementation
