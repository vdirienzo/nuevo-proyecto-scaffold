# Extra Integrations

> Additional features and integrations for your application
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for optional features that enhance your application:

- **AI Integration**: OpenAI, Anthropic, AWS Bedrock
- **Email**: SendGrid, Resend, Nodemailer
- **Feature Flags**: LaunchDarkly, Flagsmith, custom
- **Internationalization (i18n)**: next-intl, react-i18next
- **Background Jobs**: BullMQ, Celery, cron
- **Payments**: Stripe, PayPal
- **Search**: Elasticsearch, Algolia, MeiliSearch

---

## Directory Structure

```
extras/
├── ai/                          # AI integrations
│   ├── openai/
│   │   ├── client.ts.tmpl
│   │   ├── streaming.ts.tmpl
│   │   └── embeddings.ts.tmpl
│   ├── anthropic/
│   │   ├── client.ts.tmpl
│   │   └── claude.ts.tmpl
│   ├── bedrock/
│   │   ├── client.py.tmpl
│   │   └── invoke.py.tmpl
│   └── langchain/
│       ├── chain.ts.tmpl
│       └── agents.ts.tmpl
│
├── email/                       # Email providers
│   ├── sendgrid/
│   │   └── client.ts.tmpl
│   ├── resend/
│   │   └── client.ts.tmpl
│   └── templates/
│       ├── welcome.html.tmpl
│       └── reset-password.html.tmpl
│
├── feature-flags/               # Feature flag services
│   ├── launchdarkly/
│   │   └── client.ts.tmpl
│   ├── flagsmith/
│   │   └── client.ts.tmpl
│   └── hooks/
│       └── use-feature.ts.tmpl
│
├── i18n/                        # Internationalization
│   ├── next-intl/
│   │   └── config.ts.tmpl
│   ├── react-i18next/
│   │   └── config.ts.tmpl
│   ├── messages/
│   │   ├── en.json.tmpl
│   │   └── es.json.tmpl
│   └── hooks/
│       └── use-translations.ts.tmpl
│
├── jobs/                        # Background jobs
│   ├── bullmq/
│   │   ├── queue.ts.tmpl
│   │   ├── workers/
│   │   │   ├── email.worker.ts.tmpl
│   │   │   └── export.worker.ts.tmpl
│   │   └── dashboard.ts.tmpl
│   ├── celery/
│   │   ├── celery.py.tmpl
│   │   └── tasks.py.tmpl
│   └── cron/
│       └── jobs.ts.tmpl
│
├── payments/                    # Payment integrations
│   ├── stripe/
│   │   ├── client.ts.tmpl
│   │   ├── webhooks.ts.tmpl
│   │   └── checkout.ts.tmpl
│   └── paypal/
│       └── client.ts.tmpl
│
└── search/                      # Search engines
    ├── elasticsearch/
    │   ├── client.ts.tmpl
    │   └── indexing.ts.tmpl
    ├── algolia/
    │   └── client.ts.tmpl
    └── meilisearch/
        └── client.ts.tmpl
```

---

## AI Integration

### OpenAI

#### Basic Client

```typescript
// ai/openai/client.ts
import OpenAI from 'openai';

export const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export async function generateText(prompt: string) {
  const completion = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    temperature: 0.7,
  });

  return completion.choices[0].message.content;
}
```

#### Streaming

```typescript
// ai/openai/streaming.ts
export async function* streamText(prompt: string) {
  const stream = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }],
    stream: true,
  });

  for await (const chunk of stream) {
    yield chunk.choices[0]?.delta?.content || '';
  }
}

// Usage in Next.js API route
export async function POST(req: Request) {
  const { prompt } = await req.json();

  const encoder = new TextEncoder();
  const stream = new ReadableStream({
    async start(controller) {
      for await (const chunk of streamText(prompt)) {
        controller.enqueue(encoder.encode(chunk));
      }
      controller.close();
    },
  });

  return new Response(stream);
}
```

#### Embeddings

```typescript
// ai/openai/embeddings.ts
export async function generateEmbedding(text: string) {
  const response = await openai.embeddings.create({
    model: 'text-embedding-3-small',
    input: text,
  });

  return response.data[0].embedding;
}

// Vector search with Postgres pgvector
export async function searchSimilar(query: string, limit = 10) {
  const embedding = await generateEmbedding(query);

  const results = await prisma.$queryRaw`
    SELECT id, content,
           1 - (embedding <=> ${embedding}::vector) AS similarity
    FROM documents
    ORDER BY similarity DESC
    LIMIT ${limit}
  `;

  return results;
}
```

### Anthropic (Claude)

```typescript
// ai/anthropic/client.ts
import Anthropic from '@anthropic-ai/sdk';

export const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

export async function generateWithClaude(prompt: string) {
  const message = await anthropic.messages.create({
    model: 'claude-3-5-sonnet-20241022',
    max_tokens: 1024,
    messages: [{ role: 'user', content: prompt }],
  });

  return message.content[0].text;
}
```

### AWS Bedrock (Python)

```python
# ai/bedrock/client.py
import boto3
import json

bedrock = boto3.client(
    service_name='bedrock-runtime',
    region_name='us-east-1'
)

def generate_with_claude(prompt: str) -> str:
    body = json.dumps({
        "anthropic_version": "bedrock-2023-05-31",
        "max_tokens": 1024,
        "messages": [{"role": "user", "content": prompt}]
    })

    response = bedrock.invoke_model(
        modelId='anthropic.claude-3-5-sonnet-20241022-v2:0',
        body=body
    )

    result = json.loads(response['body'].read())
    return result['content'][0]['text']
```

---

## Email Integration

### Resend (Recommended)

```typescript
// email/resend/client.ts
import { Resend } from 'resend';

export const resend = new Resend(process.env.RESEND_API_KEY);

export async function sendWelcomeEmail(to: string, name: string) {
  await resend.emails.send({
    from: 'noreply@yourdomain.com',
    to,
    subject: 'Welcome!',
    html: `<h1>Welcome ${name}!</h1>`,
  });
}

// With React Email templates
import { render } from '@react-email/render';
import { WelcomeEmail } from '@/emails/WelcomeEmail';

export async function sendWelcomeEmailWithTemplate(to: string, name: string) {
  const html = render(<WelcomeEmail name={name} />);

  await resend.emails.send({
    from: 'noreply@yourdomain.com',
    to,
    subject: 'Welcome!',
    html,
  });
}
```

### SendGrid

```typescript
// email/sendgrid/client.ts
import sgMail from '@sendgrid/mail';

sgMail.setApiKey(process.env.SENDGRID_API_KEY!);

export async function sendEmail(options: {
  to: string;
  subject: string;
  html: string;
}) {
  await sgMail.send({
    from: 'noreply@yourdomain.com',
    ...options,
  });
}
```

---

## Feature Flags

### Custom Hook

```typescript
// feature-flags/hooks/use-feature.ts
import { useQuery } from '@tanstack/react-query';
import { api } from '@/lib/api';

export function useFeatureFlag(flagKey: string) {
  const { data: enabled = false } = useQuery({
    queryKey: ['feature-flags', flagKey],
    queryFn: async () => {
      const { data } = await api.get(`/features/${flagKey}`);
      return data.enabled;
    },
    staleTime: 5 * 60 * 1000, // 5 minutes
  });

  return enabled;
}

// Usage
export function NewFeature() {
  const isEnabled = useFeatureFlag('new-dashboard');

  if (!isEnabled) {
    return <OldDashboard />;
  }

  return <NewDashboard />;
}
```

### LaunchDarkly

```typescript
// feature-flags/launchdarkly/client.ts
import { LDClient, initialize } from 'launchdarkly-js-client-sdk';

let ldClient: LDClient;

export async function initFeatureFlags(userId: string) {
  ldClient = initialize(
    process.env.NEXT_PUBLIC_LAUNCHDARKLY_CLIENT_ID!,
    { key: userId }
  );

  await ldClient.waitUntilReady();
}

export function useFeature(key: string, defaultValue = false) {
  return ldClient?.variation(key, defaultValue) ?? defaultValue;
}
```

---

## Internationalization (i18n)

### next-intl (Next.js)

```typescript
// i18n/next-intl/config.ts
import { getRequestConfig } from 'next-intl/server';

export default getRequestConfig(async ({ locale }) => ({
  messages: (await import(`@/messages/${locale}.json`)).default,
}));

// middleware.ts
import createMiddleware from 'next-intl/middleware';

export default createMiddleware({
  locales: ['en', 'es', 'fr'],
  defaultLocale: 'en',
});
```

### Usage

```typescript
// app/[locale]/page.tsx
import { useTranslations } from 'next-intl';

export default function HomePage() {
  const t = useTranslations('HomePage');

  return (
    <div>
      <h1>{t('title')}</h1>
      <p>{t('description')}</p>
    </div>
  );
}

// messages/en.json
{
  "HomePage": {
    "title": "Welcome",
    "description": "This is the home page"
  }
}
```

---

## Background Jobs

### BullMQ (Node.js)

```typescript
// jobs/bullmq/queue.ts
import { Queue, Worker } from 'bullmq';
import IORedis from 'ioredis';

const connection = new IORedis({
  host: process.env.REDIS_HOST,
  port: parseInt(process.env.REDIS_PORT!),
});

// Create queue
export const emailQueue = new Queue('email', { connection });

// Add job
export async function sendEmailJob(to: string, subject: string, html: string) {
  await emailQueue.add('send-email', { to, subject, html });
}

// Worker
export const emailWorker = new Worker(
  'email',
  async (job) => {
    const { to, subject, html } = job.data;
    await sendEmail({ to, subject, html });
  },
  { connection }
);
```

### Celery (Python)

```python
# jobs/celery/celery.py
from celery import Celery

app = Celery(
    'tasks',
    broker='redis://localhost:6379/0',
    backend='redis://localhost:6379/0'
)

# tasks.py
from .celery import app

@app.task
def send_email_task(to: str, subject: str, html: str):
    send_email(to, subject, html)

# Usage
send_email_task.delay('user@example.com', 'Hello', '<p>Hi!</p>')
```

---

## Payments

### Stripe

```typescript
// payments/stripe/client.ts
import Stripe from 'stripe';

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: '2023-10-16',
});

// Create checkout session
export async function createCheckoutSession(priceId: string, userId: string) {
  const session = await stripe.checkout.sessions.create({
    line_items: [{ price: priceId, quantity: 1 }],
    mode: 'subscription',
    success_url: `${process.env.APP_URL}/success`,
    cancel_url: `${process.env.APP_URL}/cancel`,
    client_reference_id: userId,
  });

  return session;
}

// Webhook handler
export async function handleWebhook(body: string, signature: string) {
  const event = stripe.webhooks.constructEvent(
    body,
    signature,
    process.env.STRIPE_WEBHOOK_SECRET!
  );

  switch (event.type) {
    case 'checkout.session.completed':
      const session = event.data.object;
      await fulfillOrder(session);
      break;
    case 'invoice.payment_succeeded':
      // Handle successful payment
      break;
  }
}
```

---

## Search

### MeiliSearch

```typescript
// search/meilisearch/client.ts
import { MeiliSearch } from 'meilisearch';

export const meili = new MeiliSearch({
  host: process.env.MEILISEARCH_HOST!,
  apiKey: process.env.MEILISEARCH_KEY!,
});

// Index documents
export async function indexDocuments(documents: any[]) {
  const index = meili.index('products');
  await index.addDocuments(documents);
}

// Search
export async function searchProducts(query: string) {
  const index = meili.index('products');
  const results = await index.search(query, {
    limit: 20,
    attributesToHighlight: ['name', 'description'],
  });

  return results.hits;
}
```

### Algolia

```typescript
// search/algolia/client.ts
import algoliasearch from 'algoliasearch';

const client = algoliasearch(
  process.env.ALGOLIA_APP_ID!,
  process.env.ALGOLIA_API_KEY!
);

const index = client.initIndex('products');

export async function search(query: string) {
  const { hits } = await index.search(query);
  return hits;
}
```

---

## Environment Variables

```bash
# AI
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-...

# Email
RESEND_API_KEY=re_...
SENDGRID_API_KEY=SG...

# Feature Flags
LAUNCHDARKLY_SDK_KEY=...
FLAGSMITH_API_KEY=...

# Jobs
REDIS_HOST=localhost
REDIS_PORT=6379

# Payments
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Search
MEILISEARCH_HOST=http://localhost:7700
MEILISEARCH_KEY=...
ALGOLIA_APP_ID=...
ALGOLIA_API_KEY=...
```

---

## Usage Instructions

### 1. Choose Integrations

```bash
# Copy only what you need
cp extras/ai/openai/* src/lib/ai/
cp extras/email/resend/* src/lib/email/
cp extras/payments/stripe/* src/lib/payments/
```

### 2. Install Dependencies

```bash
# AI
npm install openai @anthropic-ai/sdk

# Email
npm install resend @react-email/render

# Jobs
npm install bullmq ioredis

# Payments
npm install stripe

# Search
npm install meilisearch
```

### 3. Configure Environment

```bash
# Copy and customize
cp .env.example .env

# Add your API keys
```

### 4. Implement

Follow the examples and adapt to your needs.

---

## Best Practices

### Security

- Never commit API keys
- Use environment variables
- Validate webhooks
- Rate limit API calls

### Error Handling

- Retry failed jobs
- Log errors properly
- Handle timeouts
- Graceful degradation

### Performance

- Cache responses
- Use background jobs for slow operations
- Batch operations when possible
- Monitor usage limits

---

## Resources

### Documentation

- [OpenAI](https://platform.openai.com/docs)
- [Anthropic](https://docs.anthropic.com/)
- [Stripe](https://stripe.com/docs)
- [Resend](https://resend.com/docs)
- [BullMQ](https://docs.bullmq.io/)
- [MeiliSearch](https://docs.meilisearch.com/)

---

## Support

For issues or questions:
1. Check the service documentation
2. Review examples
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
