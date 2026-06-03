# EdTech Platform Architecture

## Overview

MissionBoard is built as a modern cloud-native application leveraging Appwrite's serverless ecosystem and Supabase for data persistence.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        React Frontend                       │
│                    (Appwrite Sites)                         │
└──────────────────────────┬──────────────────────────────────┘
                           │
                    ┌──────▼───────────┐
                    │  Appwrite SDK    │
                    │  (Client)        │
                    └──────┬───────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼─────┐   ┌────────▼────────┐  ┌────▼─────────┐
   │Appwrite   │   │ Appwrite        │  │ Appwrite     │
   │Auth       │   │ Functions       │  │ Storage      │
   │(Google)   │   │ (.NET 8)        │  │ (Files)      │
   └────┬─────┘   └────────┬────────┘  └────┬─────────┘
        │                  │                 │
        └──────────────────┼─────────────────┘
                           │
        ┌──────────────────▼──────────────────┐
        │   Supabase PostgreSQL Database      │
        │   - Users                           │
        │   - Courses                         │
        │   - Enrollments                     │
        │   - Progress                        │
        │   - Resources Metadata              │
        └─────────────────────────────────────┘
```

## Component Details

### 1. Frontend (React + Appwrite Sites)
- **Authentication Flow**: OAuth with Google via Appwrite Auth
- **API Communication**: Appwrite SDK for real-time updates
- **File Management**: Upload/download through Appwrite Storage
- **Hosting**: Deployed on Appwrite Sites

### 2. Backend (Appwrite Functions with .NET 8)
- **Serverless Execution**: Pay-per-use model
- **Key Functions**:
  - User onboarding and profile setup
  - Course creation and management
  - Enrollment processing
  - Progress tracking
  - Certificate generation
  - Email notifications

### 3. Database (Supabase PostgreSQL)
- **User Management**: Authentication details, profiles
- **Course Management**: Course structure, curriculum
- **Enrollment**: User course enrollments
- **Progress**: Learning progress and milestones
- **Resources**: Metadata for videos, PDFs, assignments

### 4. Storage (Appwrite Storage)
- **PDF Documents**: Course materials, study guides
- **Videos**: Course lectures, tutorials
- **Media Assets**: Images, thumbnails
- **File Organization**: Bucket-based structure by course/type

## Data Flow

### User Authentication
1. User clicks "Login with Google"
2. Appwrite Auth handles OAuth handshake
3. User profile synced to Supabase
4. Frontend receives session token

### Course Enrollment
1. React frontend sends enrollment request
2. Appwrite Function validates and processes
3. Creates enrollment record in Supabase
4. Returns enrollment confirmation

### Resource Upload
1. Instructor uploads file via React UI
2. File sent to Appwrite Storage
3. Metadata stored in Supabase
4. Frontend updates with resource link

## Security Considerations

- **API Keys**: Stored in Appwrite console, never exposed
- **Database**: Supabase PostgreSQL with RLS policies
- **Authentication**: Industry-standard OAuth 2.0 via Google
- **File Access**: Signed URLs for temporary access
- **Rate Limiting**: Appwrite Functions built-in throttling

## Scalability

- **Serverless**: Auto-scales with demand
- **Database**: Supabase auto-scaling
- **Storage**: Unlimited file storage
- **CDN**: Appwrite Sites includes global CDN

## Deployment Pipeline

1. **Frontend**: Push to main → Appwrite Sites auto-deploy
2. **Backend**: Deploy .NET 8 functions to Appwrite
3. **Database**: Migrations via Supabase CLI
4. **Configuration**: Environment management via Appwrite console
