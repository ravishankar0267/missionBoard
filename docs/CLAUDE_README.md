# CLAUDE_README.md - Claude AI Context

This document provides Claude-specific context, guidelines, and best practices for working on the MissionBoard EdTech Platform project.

## 🎯 Quick Start for Claude

When working on MissionBoard tasks, Claude should:

1. **Read this context first** to understand the project
2. **Reference [AGENTS.md](AGENTS.md)** for general AI agent guidelines
3. **Check [ARCHITECTURE.md](../ARCHITECTURE.md)** for system design
4. **Review component-specific READMEs** before implementing

## 📋 Project Context

**MissionBoard** is a full-stack EdTech platform built with:
- **Frontend**: React 18 + TypeScript (Appwrite Sites)
- **Backend**: .NET 8 (Appwrite Functions)
- **Database**: Supabase PostgreSQL
- **Authentication**: Google OAuth via Appwrite
- **Storage**: Appwrite Storage for media files

## 🏗️ Architecture Summary

```
React Frontend → Appwrite SDK → Appwrite Functions (.NET 8)
                              → Appwrite Storage
                              → Supabase PostgreSQL
```

### Key Integration Points
- Frontend calls Appwrite Functions via SDK
- Backend (Functions) queries Supabase PostgreSQL
- Frontend can directly query Supabase for read-only data
- All files stored in Appwrite Storage with signed URLs

## 💡 Claude's Role on This Project

Claude excels at helping with:

### ✅ Strong Areas
- **Architecture decisions** - Suggest best practices for system design
- **Code generation** - Write boilerplate code and scaffolding
- **Documentation** - Create clear, comprehensive documentation
- **Debugging** - Analyze error logs and suggest fixes
- **Refactoring** - Improve code quality and performance
- **API design** - Design RESTful function signatures
- **Database optimization** - Suggest query optimizations
- **Best practices** - Apply industry standards

### ⚠️ Things to Be Careful With
- **Real-time testing** - Cannot actually run code
- **IDE-specific features** - Refer to [CURSOR_README.md](CURSOR_README.md) for Cursor-specific features
- **External API integrations** - Verify credentials don't get committed
- **Database migrations** - Always provide rollback scripts

## 🎓 Understanding the Codebase

### Frontend Structure
```
frontend/src/
├── components/
│   ├── Auth/              # Login, GoogleSignIn, ProtectedRoute
│   ├── Dashboard/         # User dashboard, progress tracking
│   ├── Courses/           # Browse, enroll, manage courses
│   ├── Learning/          # VideoPlayer, PDFViewer, lessons
│   └── Common/            # Header, Sidebar, Footer
├── pages/                 # Page components for routing
├── hooks/                 # useAuth, useCourses, useProgress
├── context/              # AuthContext, ThemeContext
├── services/             # appwrite.ts, supabase.ts, api.ts
└── utils/                # Helpers, formatters, validators
```

### Backend Structure
```
backend/
├── Functions/
│   ├── Auth/              # UserOnboarding, GoogleOAuth
│   ├── Courses/           # Create, enroll, list
│   ├── Progress/          # Track, calculate certificates
│   └── Notifications/     # Email, in-app
├── Models/               # User, Course, Enrollment models
├── Services/             # Business logic
└── Data/                 # DbContext
```

### Database Structure (9 Tables)
```
users, courses, course_modules, lessons
resources, enrollments, progress, certificates, quiz_results
```

## 📚 Key Files to Reference

| File | Purpose |
|------|---------|
| [ARCHITECTURE.md](../ARCHITECTURE.md) | System design & diagrams |
| [AGENTS.md](AGENTS.md) | AI agent guidelines |
| [DATABASE_SCHEMA.md](../infrastructure/DATABASE_SCHEMA.md) | Complete SQL schema |
| [APPWRITE_CONFIG.md](../infrastructure/APPWRITE_CONFIG.md) | Backend setup |
| [SUPABASE_CONFIG.md](../infrastructure/SUPABASE_CONFIG.md) | Database setup |
| [backend/README.md](../backend/README.md) | Backend project structure |
| [frontend/README.md](../frontend/README.md) | Frontend project structure |

## 🔧 Common Claude Tasks

### Task 1: Generate Backend Function

When Claude is asked to create a new Appwrite Function:

```
Steps:
1. Ask clarifying questions:
   - What is the function purpose?
   - What inputs does it receive?
   - What database tables does it access?
   - What errors might occur?

2. Generate the function template:
   - Use async/await pattern
   - Include try-catch for error handling
   - Return standardized response: { success, data, error }
   - Add logging

3. Include documentation:
   - XML comments explaining the function
   - Example request/response
   - Error scenarios

4. Consider:
   - Database access patterns
   - Performance implications
   - Security (user permissions)
   - Error handling
```

### Task 2: Build React Component

When Claude builds a React component:

```
Steps:
1. Define TypeScript interfaces:
   - Component props
   - State types
   - API response types

2. Create component structure:
   - Use functional components with hooks
   - Custom hooks for logic (useAuth, useCourses)
   - Proper error handling

3. Add styling:
   - Use Tailwind CSS classes
   - Mobile-responsive design
   - Accessibility features

4. Include documentation:
   - Component purpose
   - Props explanation
   - Usage example

5. Consider:
   - Performance (memoization, lazy loading)
   - Accessibility (ARIA labels)
   - Error states
   - Loading states
```

### Task 3: Design Database Migration

When Claude suggests database changes:

```
Steps:
1. Understand current schema:
   - Review DATABASE_SCHEMA.md
   - Check existing relationships
   - Identify constraints

2. Design migration:
   - Write migration SQL
   - Include rollback script
   - Consider data transformation

3. Update documentation:
   - Update DATABASE_SCHEMA.md
   - Add comments explaining changes
   - Note any breaking changes

4. Consider:
   - Data migration strategy
   - Backward compatibility
   - Performance impact
   - Foreign key constraints
```

### Task 4: Debug an Issue

When Claude helps debug:

```
Steps:
1. Gather information:
   - What is the exact error?
   - When does it occur?
   - What was the last working state?

2. Analyze:
   - Check logs (frontend console, Appwrite Function logs)
   - Review relevant code
   - Check database queries

3. Suggest fixes:
   - Propose code changes with explanation
   - Explain the root cause
   - Suggest prevention measures

4. Provide testing steps:
   - How to reproduce the issue
   - How to verify the fix
   - Edge cases to test
```

## 🔐 Security Guidelines for Claude

When working on security-related features:

### Authentication
- Use Appwrite Auth for OAuth flow
- Store tokens securely (httpOnly cookies)
- Validate tokens on every request
- Implement logout/session management

### Authorization
- Use Supabase RLS policies
- Check user permissions in functions
- Verify course instructor access
- Validate enrollment before allowing access

### Data Protection
- Use HTTPS for all communications
- Encrypt sensitive data at rest
- Use signed URLs for temporary file access
- Sanitize file uploads

### Secrets Management
- Never commit API keys
- Use environment variables
- Rotate credentials regularly
- Use different keys for dev/prod

## 📖 Code Examples Claude Should Follow

### Appwrite Function Pattern (.NET 8)
```csharp
[Function]
public async Task<dynamic> EnrollCourse(
    string userId, 
    string courseId)
{
    try
    {
        // Validate inputs
        if (string.IsNullOrEmpty(userId) || string.IsNullOrEmpty(courseId))
            return new { success = false, error = "Missing required parameters" };

        // Business logic
        var enrollment = new Enrollment
        {
            UserId = Guid.Parse(userId),
            CourseId = Guid.Parse(courseId),
            Status = "active"
        };
        
        _context.Enrollments.Add(enrollment);
        await _context.SaveChangesAsync();

        // Return success
        return new { success = true, data = enrollment };
    }
    catch (Exception ex)
    {
        return new { success = false, error = ex.Message };
    }
}
```

### React Component Pattern
```typescript
interface CourseCardProps {
  course: Course;
  onEnroll: (courseId: string) => Promise<void>;
}

export const CourseCard: React.FC<CourseCardProps> = ({ course, onEnroll }) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const handleEnroll = async () => {
    try {
      setIsLoading(true);
      setError(null);
      await onEnroll(course.id);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Enrollment failed');
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="rounded-lg border border-gray-200 p-4">
      <h3 className="text-lg font-semibold">{course.title}</h3>
      <p className="text-gray-600">{course.description}</p>
      {error && <p className="text-red-500 text-sm mt-2">{error}</p>}
      <button
        onClick={handleEnroll}
        disabled={isLoading}
        className="mt-4 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 disabled:opacity-50"
      >
        {isLoading ? 'Enrolling...' : 'Enroll Now'}
      </button>
    </div>
  );
};
```

### Database Schema Pattern
```sql
CREATE TABLE new_table (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    parent_id UUID REFERENCES parent_table(id) ON DELETE CASCADE,
    field_name DATA_TYPE NOT NULL,
    optional_field DATA_TYPE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Rollback:
DROP TABLE IF EXISTS new_table;
```

## 🎯 Best Practices for Claude

### Code Quality
- Write self-documenting code
- Use meaningful variable names
- Add comments for complex logic
- Keep functions focused and small

### Performance
- Avoid N+1 database queries
- Use indexes on frequently queried columns
- Implement caching where appropriate
- Lazy load components in React

### Testing
- Suggest unit tests for business logic
- Include edge cases in suggestions
- Test error scenarios
- Validate input parameters

### Documentation
- Include examples in code comments
- Update relevant documentation files
- Document breaking changes
- Explain "why" not just "what"

## 🚀 Workflow for Claude

When assigned a task on MissionBoard:

1. **Understand the Task**
   - Read task description carefully
   - Ask clarifying questions if needed
   - Review [AGENTS.md](AGENTS.md)

2. **Review Context**
   - Check [ARCHITECTURE.md](../ARCHITECTURE.md)
   - Review relevant documentation
   - Examine existing code patterns

3. **Plan the Solution**
   - Break down into steps
   - Consider edge cases
   - Think about performance/security

4. **Implement**
   - Follow code patterns from the project
   - Write clear, documented code
   - Include error handling

5. **Document**
   - Add code comments
   - Update relevant documentation
   - Provide usage examples

6. **Hand Off**
   - Provide clear implementation notes
   - Suggest testing approach
   - Mention any dependencies or prerequisites

## 📞 Key Contacts (Reference)

- **Project**: MissionBoard EdTech Platform
- **Repository**: ravishankar0267/missionBoard
- **Branch**: feature/edtech-platform-bootstrap
- **Documentation**: See [INDEX.md](INDEX.md)

## 🔗 Related Documentation

- **For General AI Agents**: [AGENTS.md](AGENTS.md)
- **For Cursor IDE Users**: [CURSOR_README.md](CURSOR_README.md)
- **For System Design**: [ARCHITECTURE.md](../ARCHITECTURE.md)
- **For Database**: [DATABASE_SCHEMA.md](../infrastructure/DATABASE_SCHEMA.md)
- **Doc Index**: [INDEX.md](INDEX.md)

## ⚡ Quick Reference Checklist

Before implementing:
- [ ] Reviewed [AGENTS.md](AGENTS.md)?
- [ ] Checked [ARCHITECTURE.md](../ARCHITECTURE.md)?
- [ ] Understand the database structure?
- [ ] Considered error handling?
- [ ] Thought about security implications?
- [ ] Planned for testing?
- [ ] Will update documentation?

---

**Version**: 1.0  
**Last Updated**: June 2026  
**For**: Claude AI Assistant

When you encounter "X is not implemented" or similar, refer to this document and the main project documentation to understand context and requirements.
