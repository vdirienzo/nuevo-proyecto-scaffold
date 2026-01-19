# Supabase Integration Templates

> Production-ready Supabase integration for Next.js App Router projects
> Author: {{AUTHOR}}
> Project: {{PROJECT_NAME}}

---

## Overview

This directory contains complete Supabase integration templates with:

- ✅ Client and server-side Supabase clients
- ✅ Authentication with email/password and OAuth
- ✅ Row Level Security (RLS) policies
- ✅ Storage buckets with policies
- ✅ Real-time subscriptions
- ✅ React hooks for common operations
- ✅ TypeScript types generation
- ✅ Database migrations
- ✅ Automatic user profile creation

---

## File Structure

```
supabase/
├── client.ts                          # Browser-side Supabase client
├── server.ts                          # Server-side client + admin client
├── middleware.ts                      # Auth session refresh middleware
├── types.ts                           # TypeScript types from schema
├── hooks/
│   ├── use-auth.ts                   # Authentication hook
│   ├── use-realtime.ts               # Real-time subscriptions hook
│   └── use-storage.ts                # Storage operations hook
├── migrations/
│   ├── 00001_initial_schema.sql      # Initial database schema
│   └── 00002_storage_buckets.sql     # Storage buckets setup
├── functions/
│   └── handle-new-user.sql           # User profile creation trigger
├── rls-policies.sql                  # RLS policy templates
└── .env.example                      # Environment variables template
```

---

## Quick Start

### 1. Install Dependencies

```bash
npm install @supabase/supabase-js @supabase/ssr
```

### 2. Setup Environment Variables

Copy `.env.example` to `.env.local` and fill in your Supabase credentials:

```bash
cp .env.example .env.local
```

Get your credentials from: https://supabase.com/dashboard/project/_/settings/api

### 3. Copy Files to Your Project

```bash
# Copy Supabase integration to your project
cp -r supabase/ src/lib/supabase/

# Copy middleware to project root
cp middleware.ts ../../middleware.ts
```

### 4. Run Migrations

```bash
# Using Supabase CLI
supabase db push

# Or manually in Supabase SQL Editor:
# 1. Open https://supabase.com/dashboard/project/_/sql
# 2. Copy and execute migrations in order
```

### 5. Generate Types

```bash
# Generate TypeScript types from your schema
npx supabase gen types typescript --project-id your-project-id > src/lib/supabase/types.ts
```

---

## Usage Examples

### Authentication

```tsx
'use client'

import { useAuth } from '@/lib/supabase/hooks/use-auth'

export default function LoginPage() {
  const { signIn, signUp, loading } = useAuth()

  const handleLogin = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    const formData = new FormData(e.currentTarget)
    const { error } = await signIn(
      formData.get('email') as string,
      formData.get('password') as string
    )
    if (error) console.error(error)
  }

  return <form onSubmit={handleLogin}>{/* ... */}</form>
}
```

### Server-Side Data Fetching

```tsx
import { createServerSupabaseClient } from '@/lib/supabase/server'

export default async function ProfilePage() {
  const supabase = await createServerSupabaseClient()
  const { data: profile } = await supabase
    .from('profiles')
    .select('*')
    .single()

  return <div>{profile?.full_name}</div>
}
```

### Real-time Subscriptions

```tsx
'use client'

import { useRealtime } from '@/lib/supabase/hooks/use-realtime'

export default function MessagesComponent() {
  const change = useRealtime({
    table: 'messages',
    event: 'INSERT',
    filter: 'room_id=eq.123'
  })

  useEffect(() => {
    if (change?.new) {
      // Handle new message
      console.log('New message:', change.new)
    }
  }, [change])

  return <div>{/* ... */}</div>
}
```

### File Upload

```tsx
'use client'

import { useStorage } from '@/lib/supabase/hooks/use-storage'

export default function AvatarUpload() {
  const { upload, uploading, progress } = useStorage()

  const handleUpload = async (file: File) => {
    const { data, error } = await upload({
      bucket: 'avatars',
      path: `${userId}/avatar.png`,
      file,
      upsert: true
    })
  }

  return (
    <div>
      {uploading && <progress value={progress} max={100} />}
    </div>
  )
}
```

---

## Security Best Practices

### 1. Never Expose Service Role Key

```tsx
// ❌ NEVER do this
const supabase = createClient(url, process.env.SUPABASE_SERVICE_ROLE_KEY!)

// ✅ Use anon key for browser
const supabase = createClient(url, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!)

// ✅ Use service role only in server-side code
import { createServerSupabaseAdminClient } from '@/lib/supabase/server'
const admin = createServerSupabaseAdminClient()
```

### 2. Always Use Row Level Security

```sql
-- Enable RLS on all tables
ALTER TABLE your_table ENABLE ROW LEVEL SECURITY;

-- Create appropriate policies
CREATE POLICY "Users can view their own data"
  ON your_table FOR SELECT
  USING (auth.uid() = user_id);
```

### 3. Validate File Uploads

```tsx
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/webp']
const MAX_SIZE = 5 * 1024 * 1024 // 5MB

if (!ALLOWED_TYPES.includes(file.type)) {
  throw new Error('Invalid file type')
}

if (file.size > MAX_SIZE) {
  throw new Error('File too large')
}
```

### 4. Use Storage Policies

```sql
-- Only allow users to upload to their own folder
CREATE POLICY "Users can upload their own files"
  ON storage.objects FOR INSERT
  WITH CHECK (
    bucket_id = 'avatars'
    AND (storage.foldername(name))[1] = auth.uid()::text
  );
```

---

## Database Migrations

### Creating New Migrations

1. Create a new file in `migrations/` with incrementing number:
   ```
   migrations/00003_add_posts_table.sql
   ```

2. Write your migration:
   ```sql
   CREATE TABLE posts (
     id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
     user_id UUID REFERENCES auth.users(id),
     title TEXT NOT NULL,
     content TEXT,
     created_at TIMESTAMPTZ DEFAULT NOW()
   );

   ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
   ```

3. Run migration:
   ```bash
   supabase db push
   ```

4. Regenerate types:
   ```bash
   npx supabase gen types typescript --project-id your-project-id > types.ts
   ```

---

## Testing

### Local Development with Supabase CLI

```bash
# Install Supabase CLI
npm install -g supabase

# Start local Supabase
supabase start

# Apply migrations
supabase db push

# Get local credentials
supabase status
```

Update `.env.local`:
```
NEXT_PUBLIC_SUPABASE_URL=http://localhost:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=<local-anon-key>
```

---

## Troubleshooting

### Issue: RLS policies blocking queries

**Solution:** Check policies with:
```sql
-- Test as specific user
SET LOCAL request.jwt.claims = '{"sub": "user-uuid"}';
SELECT * FROM your_table;
```

### Issue: Storage upload fails

**Solution:** Verify:
1. Bucket exists and is configured
2. Storage policies allow the operation
3. File size/type is within limits

### Issue: Types are outdated

**Solution:** Regenerate types:
```bash
npx supabase gen types typescript --project-id your-project-id > types.ts
```

---

## Resources

- [Supabase Documentation](https://supabase.com/docs)
- [Supabase Auth](https://supabase.com/docs/guides/auth)
- [Row Level Security](https://supabase.com/docs/guides/auth/row-level-security)
- [Storage](https://supabase.com/docs/guides/storage)
- [Realtime](https://supabase.com/docs/guides/realtime)

---

## Changelog

### [1.0.0] - 2025-01-19

#### Added
- Initial Supabase integration templates
- Authentication hooks (email/password, OAuth)
- Real-time subscription hooks
- Storage management hooks
- Server and client-side Supabase clients
- Row Level Security policies
- Database migrations
- Automatic user profile creation
- TypeScript types generation
- Environment variables template

---

## Author

{{AUTHOR}}
