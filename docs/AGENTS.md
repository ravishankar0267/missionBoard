# AGENTS.md - AI Agent Context Guide

This document provides context for AI coding agents (GitHub Copilot, Claude, Cursor) working on the MissionBoard EdTech Platform.

## 📋 Project Overview

**MissionBoard** is a cloud-native EdTech platform that combines:
- **Frontend**: React 18 with TypeScript running on Appwrite Sites
- **Backend**: .NET 8 serverless functions on Appwrite Functions
- **Database**: Supabase PostgreSQL with Row-Level Security
- **Authentication**: Google OAuth via Appwrite Auth
- **Storage**: Appwrite Storage for PDFs, videos, and media files

## 🎯 Core Purpose

Build a scalable, cloud-native education platform where:
- Students can enroll in courses
- Instructors can create and manage courses
- Progress is tracked automatically
- Certificates are generated upon completion
- All resources (videos, PDFs) are securely stored

## 📊 System Architecture

```
User (Browser)
    ↓
React Frontend (Appwrite Sites)
    ├→ Appwrite SDK (Auth, Storage)
    └→ Supabase Client
         ├→ User Data
         ├→ Course Info
         ├→ Progress
         └→ Enrollments
         
Appwrite Functions (.NET 8)
    ├→ User Onboarding
    ├→ Course Management
    ├→ Progress Tracking
    └→ Certificate Generation
         ↓
    Supabase PostgreSQL
    + Appwrite Storage
```

## 🗂️ Repository Structure

```
missionBoard/
├── backend/                      # .NET 8 backend
│   ├── Functions/               # Serverless functions
│   │   ├── Auth/               # Authentication handlers
│   │   ├── Courses/            # Course management
│   │   ├── Progress/           # Progress tracking
│   │   └── Notifications/      # Email & notifications
│   ├── Models/                 # Data models
│   ├── Data/                   # DbContext
│   ├── Services/               # Business logic
│   └── missionboard.csproj
│
├── frontend/                    # React application
│   ├── src/
│   │   ├── components/         # React components (Auth, Courses, Learning)
│   │   ├── pages/              # Page routes
│   │   ├── hooks/              # useAuth, useCourses, useProgress
│   │   ├── context/            # AuthContext, ThemeContext
│   │   ├── services/           # appwrite.ts, supabase.ts, api.ts
│   │   ├── styles/             # Tailwind config
│   │   └── utils/              # Helper functions
│   └── package.json
│
├── infrastructure/              # Infrastructure & config
│   ├── DATABASE_SCHEMA.md       # PostgreSQL schema
│   ├── APPWRITE_CONFIG.md       # Appwrite setup guide
│   └── SUPABASE_CONFIG.md       # Supabase setup guide
│
├── docs/                        # Documentation
│   ├── INDEX.md                 # Doc index
│   ├── AGENTS.md               # This file
│   ├── CLAUDE_README.md         # Claude context
│   └── CURSOR_README.md         # Cursor context
│
├── ARCHITECTURE.md              # System design
└── README.md                    # Main README
```

## 🔑 Key Technologies

### Frontend Stack
- **React 18**: UI framework
- **TypeScript**: Type safety
- **Tailwind CSS**: Styling
- **React Query**: Data fetching & caching
- **Appwrite SDK**: Backend integration
- **Supabase JS**: Database client

### Backend Stack
- **.NET 8**: Runtime
- **C#**: Language
- **Entity Framework Core**: ORM
- **Npgsql**: PostgreSQL driver
- **Appwrite SDK**: Platform integration

### Database
- **PostgreSQL 15+**: Relational database
- **Supabase**: Managed PostgreSQL hosting
- **9 Core Tables**: users, courses, modules, lessons, resources, enrollments, progress, certificates, quiz_results

## 📚 Database Schema Overview

```
users (id, email, full_name, role, google_id)
    ↓ (instructor)
courses (id, title, description, instructor_id, category, level)
    ↓
course_modules (id, course_id, title, order_index)
    ↓
lessons (id, module_id, title, lesson_type, content_url)
    ├→ resources (id, lesson_id, appwrite_file_id, file_name)
    └→ quiz_results (id, user_id, lesson_id, score)

enrollments (id, user_id, course_id, status)
    ↓
progress (id, enrollment_id, lesson_id, completion_percentage)
    ↓
certificates (id, enrollment_id, certificate_number, pdf_url)
```

## 🔐 Authentication Flow

1. User clicks "Sign in with Google"
2. Appwrite Auth handles OAuth handshake
3. User profile created in Supabase (if new user)
4. Session token stored in frontend
5. Subsequent requests include token in headers

```
Google OAuth
    ↓
Appwrite Auth
    ↓
Session Token
    ↓
Supabase Auth Context
    ↓
Protected Routes
```

## 🚀 Common Development Workflows

### Adding a New Feature

1. **Plan the feature** in Appwrite (backend function) or React (frontend component)
2. **Update database schema** if needed (Supabase migrations)
3. **Create API endpoint** (Appwrite Function in .NET 8)
4. **Build React component** (TypeScript + Tailwind)
5. **Add routing** if it's a new page
6. **Test with real data** from Supabase

### Adding a New Course Field

1. Add column to `courses` table in Supabase
2. Update Course model in .NET backend
3. Create database migration
4. Update Appwrite Function response
5. Update React component to display/edit field
6. Update TypeScript interfaces

### Adding Storage (PDF/Video)

1. Upload file to Appwrite Storage bucket
2. Store file metadata in Supabase `resources` table
3. Generate signed URL in Appwrite Function
4. Display in React component with VideoPlayer/PDFViewer

## 💻 Development Guidelines

### Code Structure
- **Backend**: Organize by feature (Auth, Courses, Progress)
- **Frontend**: Organize by component type (Auth, Dashboard, Courses)
- **Database**: Use migrations for schema changes
- **Types**: Use TypeScript interfaces for all data

### Naming Conventions
- **Functions**: `PascalCase` (.NET), `camelCase` (TypeScript)
- **Variables**: `camelCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Components**: `PascalCase`
- **Files**: `kebab-case` (TypeScript), `PascalCase.cs` (.NET)

### API Endpoints Pattern
All Appwrite Functions follow this pattern:

```
POST /functions/{functionId}
Request: { user_id, course_id, data }
Response: { success, data, error }
```

### Error Handling
- Backend: Throw exceptions, return error response
- Frontend: Catch errors, display user-friendly message
- Database: RLS policies prevent unauthorized access

## 🔗 Integration Points

### Frontend ↔ Backend Communication

```typescript
// Frontend calls Appwrite Function
const response = await client
  .call(execution.create("function-id", JSON.stringify({
    action: "enroll",
    course_id: "123",
    user_id: "456"
  })));
```

### Backend ↔ Database Communication

```csharp
// Backend queries Supabase via EF Core
var enrollments = await dbContext.Enrollments
  .Where(e => e.UserId == userId)
  .ToListAsync();
```

### Frontend ↔ Database (Direct)

```typescript
// Frontend queries Supabase directly
const { data, error } = await supabase
  .from('courses')
  .select('*')
  .eq('is_published', true);
```

## 🎯 Common Tasks for AI Agents

### Task: Create New Appwrite Function
1. Create file: `backend/Functions/{Category}/{FunctionName}.cs`
2. Implement handler with proper error handling
3. Register in `Program.cs` if needed
4. Document the function signature
5. Test with sample data

### Task: Add React Component
1. Create component file in `frontend/src/components/{Category}/{ComponentName}.tsx`
2. Define TypeScript interfaces for props
3. Use hooks (useState, useEffect, custom hooks)
4. Style with Tailwind classes
5. Export and import where needed

### Task: Database Migration
1. Create migration file with timestamp: `migrations/{timestamp}_{description}.sql`
2. Write SQL for schema changes
3. Include rollback SQL
4. Update `DATABASE_SCHEMA.md`
5. Test locally before deploying

### Task: Fix Bug
1. Reproduce bug with specific steps
2. Check logs: Frontend console or Appwrite Function logs
3. Check data: Query Supabase database directly
4. Check authentication: Verify token is valid
5. Test fix in development before committing

## 📖 Reference Documents

| Document | Purpose |
|----------|---------|
| [ARCHITECTURE.md](../ARCHITECTURE.md) | System design & data flow |
| [DATABASE_SCHEMA.md](../infrastructure/DATABASE_SCHEMA.md) | Database structure |
| [APPWRITE_CONFIG.md](../infrastructure/APPWRITE_CONFIG.md) | Appwrite setup |
| [SUPABASE_CONFIG.md](../infrastructure/SUPABASE_CONFIG.md) | Supabase setup |
| [backend/README.md](../backend/README.md) | Backend project setup |
| [frontend/README.md](../frontend/README.md) | Frontend project setup |

## 🤔 Troubleshooting Guide

### Frontend Issue
1. Check browser console for errors
2. Check Network tab for failed requests
3. Verify Appwrite/Supabase credentials in .env
4. Check authentication token validity
5. Verify RLS policies allow the operation

### Backend Issue
1. Check Appwrite Function logs
2. Verify database connection string
3. Check Entity Framework migrations
4. Verify user permissions
5. Test with Postman or cURL

### Database Issue
1. Query directly in Supabase dashboard
2. Check RLS policies are not blocking
3. Verify indexes for performance
4. Check foreign key constraints
5. Review recent migrations

## 🚦 Code Quality Standards

- **Frontend**: ESLint, TypeScript strict mode
- **Backend**: StyleCop, null-safety enabled
- **Database**: Proper indexing, constraints
- **Documentation**: Updated with code changes
- **Tests**: Unit tests for critical functions

## 📝 Commit Message Format

```
feat: Add user authentication flow
fix: Resolve course enrollment issue
docs: Update database schema documentation
refactor: Simplify progress calculation
test: Add unit tests for auth service
```

## 🎓 Learning Path for New Agents

1. **Start**: Read [ARCHITECTURE.md](../ARCHITECTURE.md)
2. **Understand**: Review [DATABASE_SCHEMA.md](../infrastructure/DATABASE_SCHEMA.md)
3. **Setup**: Follow [backend/README.md](../backend/README.md) or [frontend/README.md](../frontend/README.md)
4. **Explore**: Browse the codebase structure
5. **Contribute**: Start with small features
6. **Grow**: Move to complex features and optimizations

## 🔄 Development Loop

```
1. Create feature branch
2. Implement changes
3. Test locally
4. Create pull request
5. Review with team
6. Merge to main
7. Deploy to production
```

## ⚡ Performance Considerations

- **Frontend**: Lazy load components, memoize expensive computations
- **Backend**: Use async/await, batch database queries
- **Database**: Use indexes on frequently queried columns
- **Storage**: Compress media files, use CDN for delivery
- **Caching**: Use React Query for client-side caching

## 🔒 Security Best Practices

- Never commit API keys or secrets
- Use environment variables for configuration
- Validate all user inputs
- Use Supabase RLS policies for data access
- Sanitize file uploads
- Use HTTPS for all communications
- Rotate credentials regularly

---

**Version**: 1.0  
**Last Updated**: June 2026  
**Maintained By**: Development Team

For questions or clarifications, refer to the [INDEX.md](INDEX.md) or specific component READMEs.
