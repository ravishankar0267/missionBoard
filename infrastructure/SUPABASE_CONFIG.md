# Supabase Configuration

## Project Setup

### Create Supabase Project

1. Sign up at https://supabase.com
2. Create new project
3. Save Project URL and API Keys

### API Keys

- **Public (Anon) Key**: For client-side operations
- **Service Role Key**: For server-side/function operations (keep secret)

## Database Configuration

### Extensions

Enable useful PostgreSQL extensions:

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";
```

### Connection

```typescript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.REACT_APP_SUPABASE_URL,
  process.env.REACT_APP_SUPABASE_ANON_KEY
)
```

## Authentication

### Setup Email Provider

1. Go to Authentication → Providers
2. Enable Email provider
3. Configure email settings if using custom SMTP

### Google OAuth

1. Create OAuth credentials at Google Cloud Console
2. In Supabase: Authentication → Providers → Google
3. Add Client ID and Client Secret

## Row Level Security (RLS)

### Enable RLS on Tables

```sql
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE enrollments ENABLE ROW LEVEL SECURITY;
ALTER TABLE progress ENABLE ROW LEVEL SECURITY;
```

### Example Policies

```sql
-- Users can only view their own profile
CREATE POLICY "Users can view own profile" ON users
    FOR SELECT
    USING (auth.uid()::text = id::text);

-- Users can only update their own profile
CREATE POLICY "Users can update own profile" ON users
    FOR UPDATE
    USING (auth.uid()::text = id::text);

-- Users can view their own enrollments
CREATE POLICY "Users can view enrollments" ON enrollments
    FOR SELECT
    USING (auth.uid()::text = user_id::text);

-- Instructors can view all enrollments in their courses
CREATE POLICY "Instructors can view enrollments" ON enrollments
    FOR SELECT
    USING (
        EXISTS (
            SELECT 1 FROM courses
            WHERE courses.id = enrollments.course_id
            AND courses.instructor_id::text = auth.uid()::text
        )
    );
```

## Migrations

### Using Supabase CLI

```bash
# Initialize Supabase locally
supabase init

# Pull existing schema
supabase db pull

# Create new migration
supabase migration new create_users_table

# Edit migration file and run
supabase db push

# Deploy to production
supabase db push --linked
```

## Replication

### Real-time Subscriptions

```typescript
const subscription = supabase
  .from('progress')
  .on('*', (payload) => {
    console.log('Change received!', payload)
  })
  .subscribe()
```

## Backups

### Automatic Backups
- Daily backups are automatically created
- Retention: 7 days for free tier
- Available in Supabase Dashboard → Backups

### Manual Backups

```bash
# Export database
supabase db dump > backup.sql
```

## Performance

### Indexing

Key indexes already defined in DATABASE_SCHEMA.md

### Query Optimization

```typescript
// Use select to limit columns
const { data } = await supabase
  .from('courses')
  .select('id, title, thumbnail_url')
  .limit(10)
```

### Connection Pooling

Supabase provides automatic connection pooling. Configure if needed:
- Session pool mode: Good for applications
- Transaction pool mode: Good for read-heavy workloads

## Monitoring

### Query Logs
View in Supabase Dashboard → Database → Logs → Query Performance

### Realtime Logs
Monitor real-time subscriptions in Dashboard → Realtime

## Scaling

### Horizontal Scaling
- Supabase handles auto-scaling
- Monitor database size in Settings
- Upgrade plan if needed for higher limits

### Vertical Scaling
- Upgrade project tier for more compute
- Available in Settings → Upgrade Plan
