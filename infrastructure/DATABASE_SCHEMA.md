# Database Schema - Supabase PostgreSQL

## Overview

This document describes the database schema for the MissionBoard edTech platform.

## Tables

### 1. users
User account and profile information.

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    profile_image_url TEXT,
    bio TEXT,
    role VARCHAR(50) DEFAULT 'student', -- 'student', 'instructor', 'admin'
    google_id VARCHAR(255),
    appwrite_user_id VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    last_login TIMESTAMP
);
```

### 2. courses
Course information and metadata.

```sql
CREATE TABLE courses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    instructor_id UUID REFERENCES users(id),
    category VARCHAR(100),
    level VARCHAR(50), -- 'beginner', 'intermediate', 'advanced'
    duration_hours INT,
    thumbnail_url TEXT,
    is_published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### 3. course_modules
Structured course content in modules.

```sql
CREATE TABLE course_modules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    course_id UUID REFERENCES courses(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    order_index INT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### 4. lessons
Individual lessons within modules.

```sql
CREATE TABLE lessons (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    module_id UUID REFERENCES course_modules(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    lesson_type VARCHAR(50), -- 'video', 'pdf', 'quiz', 'assignment'
    content_url TEXT,
    duration_minutes INT,
    order_index INT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### 5. resources
Metadata for stored files (videos, PDFs).

```sql
CREATE TABLE resources (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    lesson_id UUID REFERENCES lessons(id) ON DELETE CASCADE,
    resource_type VARCHAR(50), -- 'video', 'pdf', 'image'
    appwrite_bucket_id VARCHAR(255),
    appwrite_file_id VARCHAR(255),
    file_name VARCHAR(255),
    file_size_bytes BIGINT,
    mime_type VARCHAR(100),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### 6. enrollments
User course enrollments.

```sql
CREATE TABLE enrollments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    course_id UUID REFERENCES courses(id) ON DELETE CASCADE,
    status VARCHAR(50) DEFAULT 'active', -- 'active', 'completed', 'dropped'
    enrolled_at TIMESTAMP DEFAULT NOW(),
    completed_at TIMESTAMP,
    UNIQUE(user_id, course_id)
);
```

### 7. progress
User learning progress tracking.

```sql
CREATE TABLE progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID REFERENCES enrollments(id) ON DELETE CASCADE,
    lesson_id UUID REFERENCES lessons(id),
    completion_percentage INT DEFAULT 0,
    completed_at TIMESTAMP,
    time_spent_seconds INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### 8. certificates
Generated course completion certificates.

```sql
CREATE TABLE certificates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID REFERENCES enrollments(id) ON DELETE CASCADE,
    certificate_number VARCHAR(255) UNIQUE,
    issued_at TIMESTAMP DEFAULT NOW(),
    pdf_url TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);
```

### 9. quiz_results
User quiz submission results.

```sql
CREATE TABLE quiz_results (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    lesson_id UUID REFERENCES lessons(id) ON DELETE CASCADE,
    score INT,
    total_points INT,
    passed BOOLEAN,
    submitted_at TIMESTAMP DEFAULT NOW()
);
```

## Indexes

```sql
CREATE INDEX idx_courses_instructor ON courses(instructor_id);
CREATE INDEX idx_enrollments_user ON enrollments(user_id);
CREATE INDEX idx_enrollments_course ON enrollments(course_id);
CREATE INDEX idx_progress_enrollment ON progress(enrollment_id);
CREATE INDEX idx_resources_lesson ON resources(lesson_id);
CREATE INDEX idx_certificates_enrollment ON certificates(enrollment_id);
```

## Row Level Security (RLS)

### Users Table
```sql
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own profile"
    ON users FOR SELECT
    USING (auth.uid() = id);

CREATE POLICY "Users can update own profile"
    ON users FOR UPDATE
    USING (auth.uid() = id);
```

### Enrollments Table
```sql
ALTER TABLE enrollments ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own enrollments"
    ON enrollments FOR SELECT
    USING (auth.uid() = user_id);
```

### Progress Table
```sql
ALTER TABLE progress ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own progress"
    ON progress FOR SELECT
    USING (
        auth.uid() = (
            SELECT user_id FROM enrollments WHERE id = progress.enrollment_id
        )
    );
```

## Migrations

Run migrations using Supabase CLI:
```bash
supabase migration up
```

## Backup

Automatic daily backups are configured in Supabase. Manual backups can be taken from the dashboard.
