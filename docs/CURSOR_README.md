# CURSOR_README.md - Cursor IDE Context

This document provides Cursor IDE-specific setup, extensions, and best practices for working on the MissionBoard EdTech Platform.

## 🚀 Quick Start for Cursor Users

1. **Open the project** in Cursor
2. **Read this document** for IDE setup
3. **Read [AGENTS.md](AGENTS.md)** for project guidelines
4. **Read [ARCHITECTURE.md](../ARCHITECTURE.md)** for system design

## 📋 Cursor Setup for MissionBoard

### Initial Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/ravishankar0267/missionBoard.git
   cd missionBoard
   git checkout feature/edtech-platform-bootstrap
   ```

2. **Open in Cursor**
   ```bash
   cursor .
   ```

3. **Install recommended extensions** (see below)

### Recommended Cursor Extensions

#### Essential
- **TypeScript Vue Plugin (Volar)** - Vue/TypeScript support
- **Prettier - Code formatter** - Code formatting
- **ESLint** - JavaScript/TypeScript linting
- **Git Graph** - Git visualization

#### Frontend Development
- **React Components** - React component snippets
- **Tailwind CSS IntelliSense** - Tailwind autocompletion
- **Thunder Client** or **REST Client** - API testing

#### Backend Development (.NET)
- **C# Dev Kit** - Full C# support
- **vscode-nuget-package-manager** - NuGet package management
- **.NET Core Tools** - .NET SDK integration

#### Database
- **PostgreSQL Explorer** - Database connection
- **SQLTools** - SQL query execution
- **Prisma** - Database migration tools

#### Documentation
- **Markdown All in One** - Markdown editing
- **Markdown Preview Enhanced** - Enhanced markdown preview

### Cursor Settings (settings.json)

Add these to your Cursor `settings.json`:

```json
{
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
  },
  "[csharp]": {
    "editor.defaultFormatter": "ms-dotnettools.csharp",
    "editor.formatOnSave": true
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  },
  "git.ignoreLimitWarning": true,
  "search.exclude": {
    "**/node_modules": true,
    "**/bin": true,
    "**/obj": true
  },
  "files.watcherExclude": {
    "**/node_modules": true,
    "**/bin": true,
    "**/obj": true
  },
  "[sql]": {
    "editor.defaultFormatter": "mtxr.sqltools"
  }
}
```

### Cursor Keybindings for MissionBoard

Add these useful keybindings to `keybindings.json`:

```json
[
  {
    "key": "ctrl+shift+f",
    "command": "editor.action.formatDocument"
  },
  {
    "key": "ctrl+k ctrl+x",
    "command": "editor.action.trimTrailingWhitespace"
  },
  {
    "key": "f12",
    "command": "editor.action.goToDeclaration"
  }
]
```

## 🎯 Project Structure Navigation in Cursor

### Frontend Navigation (Ctrl+P or Cmd+P)

Useful file shortcuts:
```
# Components
Ctrl+P > "CourseCard" → frontend/src/components/Courses/CourseCard.tsx
Ctrl+P > "Dashboard" → frontend/src/pages/DashboardPage.tsx
Ctrl+P > "useAuth" → frontend/src/hooks/useAuth.ts

# Services
Ctrl+P > "appwrite.ts" → frontend/src/services/appwrite.ts
Ctrl+P > "supabase.ts" → frontend/src/services/supabase.ts

# Styles
Ctrl+P > "tailwind" → frontend/tailwind.config.js
```

### Backend Navigation

```
# Functions
Ctrl+P > "CourseCreate.cs" → backend/Functions/Courses/CourseCreate.cs
Ctrl+P > "UserOnboarding" → backend/Functions/Auth/UserOnboarding.cs

# Models
Ctrl+P > "Course.cs" → backend/Models/Course.cs
Ctrl+P > "User.cs" → backend/Models/User.cs

# Services
Ctrl+P > "CourseService" → backend/Services/CourseService.cs
```

### Documentation Navigation

```
Ctrl+P > "INDEX.md" → docs/INDEX.md (Documentation index)
Ctrl+P > "ARCHITECTURE" → ARCHITECTURE.md (System design)
Ctrl+P > "DATABASE_SCHEMA" → infrastructure/DATABASE_SCHEMA.md
Ctrl+P > "AGENTS.md" → docs/AGENTS.md (AI agent context)
```

## 🔍 Cursor-Specific Features to Use

### 1. Cursor AI Features

#### Code Generation
Press `Ctrl+K` to generate code inline:
```typescript
// Type: Create a function to validate email
// Cursor generates:
const validateEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};
```

#### Code Explanation
Select code and press `Ctrl+L` to get explanation:
```csharp
// Select this method and ask Cursor to explain
public async Task<List<Course>> GetCourses(string userId)
{
  return await _context.Courses
    .Where(c => c.Enrollments.Any(e => e.UserId == userId))
    .ToListAsync();
}
```

#### Fix Problems
Cursor can suggest fixes for errors. Click the lightbulb or press `Ctrl+.`

### 2. Multi-File Editing

Open files side-by-side to compare:
- Component file + styled component
- Frontend service + Backend function
- Migration file + Schema documentation

### 3. Integrated Terminal

Use Cursor's integrated terminal for commands:
```bash
# Frontend
cd frontend && npm start

# Backend
cd backend && dotnet run

# Database
supabase db push
```

### 4. Source Control Integration

Use Cursor's Git panel to:
- View branch history
- Create commits with detailed messages
- Stage/unstage files
- Resolve merge conflicts

## 📁 Useful Cursor Workspace Setup

### Create a Cursor Workspace File

Create `.vscode/launch.json` for debugging:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": ".NET Core Launch (web)",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/backend/bin/Debug/net8.0/missionboard.dll",
      "args": [],
      "cwd": "${workspaceFolder}/backend",
      "stopAtEntry": false,
      "serverReadyAction": {
        "action": "openExternally",
        "pattern": "\\bNow listening on:\\s+(https?://\\S+)",
        "uriFormat": "{0}"
      },
      "env": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "sourceLanguages": ["csharp"]
    },
    {
      "name": "Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/frontend/src",
      "sourceMaps": true
    }
  ],
  "compounds": [
    {
      "name": "Full Stack Debug",
      "configurations": [".NET Core Launch (web)", "Chrome"]
    }
  ]
}
```

### Create Tasks Configuration

Create `.vscode/tasks.json` for common tasks:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Backend",
      "command": "dotnet",
      "args": ["build"],
      "cwd": "${workspaceFolder}/backend",
      "type": "shell",
      "problemMatcher": "$msCompile",
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "Install Frontend",
      "command": "npm",
      "args": ["install"],
      "cwd": "${workspaceFolder}/frontend",
      "type": "shell",
      "group": "build"
    },
    {
      "label": "Run Frontend",
      "command": "npm",
      "args": ["start"],
      "cwd": "${workspaceFolder}/frontend",
      "type": "shell",
      "isBackground": true,
      "group": "test"
    },
    {
      "label": "Run Backend",
      "command": "dotnet",
      "args": ["run"],
      "cwd": "${workspaceFolder}/backend",
      "type": "shell",
      "isBackground": true,
      "group": "test"
    }
  ]
}
```

## 🔗 Cursor Context for MissionBoard

### Project Context Files

Tell Cursor about the project context:

```
1. Read: docs/INDEX.md (documentation index)
2. Read: docs/AGENTS.md (general AI guidelines)
3. Read: ARCHITECTURE.md (system design)
4. Read: Relevant README (backend, frontend, or infra)
```

### Cursor @ Commands (Chat)

In Cursor Chat, use:
- `@codebase` - Search entire codebase
- `@docs` - Reference documentation
- `@current-file` - Current file context
- `@terminal` - Terminal output

Example in Chat:
```
@codebase How is authentication currently implemented?
@docs What is the database schema for courses?
@current-file Explain this component
```

## 🛠️ Common Cursor Workflows

### Workflow 1: Adding a New Feature

1. **Create feature branch**
   ```bash
   # In Cursor terminal
   git checkout -b feature/new-feature
   ```

2. **Create component**
   - Press `Ctrl+N` to create new file
   - Name: `NewComponent.tsx`
   - Use Cursor AI to generate structure

3. **Add styling**
   - Add Tailwind classes using autocomplete
   - Preview in browser

4. **Connect to API**
   - Reference service files
   - Use Cursor to generate fetch logic

5. **Commit and push**
   - Use Git panel to stage/commit
   - Write clear commit message

### Workflow 2: Debugging Backend Issue

1. **Set breakpoint** - Click line number in `.cs` file
2. **Start debugging** - Press F5 or use Run menu
3. **Step through code** - F10 (step over), F11 (step into)
4. **Inspect variables** - Hover or use Debug panel
5. **Use Debug Console** - Execute queries/commands

### Workflow 3: Database Migration

1. **Create migration file**
   - Name: `infrastructure/migrations/{timestamp}_{description}.sql`
   - Use Cursor to generate SQL

2. **Write migration**
   - Add forward migration SQL
   - Add rollback SQL

3. **Test locally**
   - Use Supabase CLI
   - Verify in Supabase dashboard

4. **Update documentation**
   - Edit `DATABASE_SCHEMA.md`
   - Document the changes

## 💡 Cursor Tips for MissionBoard

### 1. Code Navigation
- **Go to Definition**: F12
- **Find References**: Shift+F12
- **Open File**: Ctrl+P
- **Go to Line**: Ctrl+G

### 2. Code Editing
- **Format Document**: Shift+Alt+F
- **Toggle Comment**: Ctrl+/
- **Rename Symbol**: F2
- **Find/Replace**: Ctrl+H

### 3. Performance Tips
- **Exclude node_modules**: Already in settings
- **Exclude bin/obj**: Already in settings
- **Use workspace folders**: Open backend and frontend separately if too slow

### 4. IntelliSense Tips
- Type a few characters for autocomplete
- Press Ctrl+Space to open autocomplete
- Use Tab to accept suggestion
- Use Escape to dismiss

## 📚 Documentation in Cursor

Quick access to documentation:
1. Press `Ctrl+K` and type `docs/INDEX.md`
2. Split screen to keep docs visible
3. Reference while coding

### Key Documentation Files to Keep Open

```
Primary: ARCHITECTURE.md (system design)
Secondary: docs/AGENTS.md (guidelines)
Tertiary: Component-specific README
```

## 🔄 Cursor Git Workflow

### Recommended Git Process

1. **Create feature branch**
   ```bash
   git checkout -b feature/feature-name
   ```

2. **Make changes** (use Cursor editor)

3. **Review changes** (use Source Control panel)

4. **Stage changes**
   - Right-click files → Stage Changes
   - Or use Ctrl+K, Ctrl+S

5. **Write commit message**
   - Follow format: `feat:`, `fix:`, `docs:`, `refactor:`
   - Example: `feat: Add course enrollment functionality`

6. **Push to branch**
   ```bash
   git push origin feature/feature-name
   ```

7. **Create Pull Request** on GitHub

## ⚙️ Cursor Extensions Configuration

### For TypeScript/React (Frontend)

Install and configure:
1. ESLint - Enable in User settings
2. Prettier - Set as default formatter
3. Tailwind CSS IntelliSense - Autocomplete classes

### For C# (.NET Backend)

Install and configure:
1. C# Dev Kit - Full IDE support
2. C# XML Documentation - IntelliSense
3. NuGet Package Manager - Manage packages

### For SQL (Database)

Install and configure:
1. SQLTools - Query execution
2. PostgreSQL driver - For Supabase
3. SQL Formatter - Format SQL

## 🐛 Debugging in Cursor

### Frontend Debugging
1. Set breakpoint in `.tsx` file
2. Press F5 or use Debug menu
3. Open Chrome DevTools (F12)
4. Step through code

### Backend Debugging
1. Set breakpoint in `.cs` file
2. Press F5 to start debugging
3. Use Debug Console to inspect variables
4. Hover over variables to see values

### Database Debugging
1. Use SQLTools to connect to Supabase
2. Run queries to inspect data
3. Use Supabase dashboard for advanced queries
4. Check RLS policies in Supabase UI

## 🚀 Performance Optimization Tips

### Editor Performance
- Disable unnecessary extensions
- Use `files.watcherExclude` for large directories
- Close unused files
- Use single-folder workspace for better performance

### Development Performance
- Use development builds (faster than production)
- Enable Fast Refresh for React
- Use dotnet watch for .NET

## 📞 Help & Resources

- **Cursor Documentation**: https://cursor.sh
- **VS Code Tips**: https://code.visualstudio.com/tips-and-tricks
- **C# in VS Code**: https://github.com/omnisharp/omnisharp-vscode
- **Project Docs**: See [docs/INDEX.md](INDEX.md)

## ✅ Cursor Checklist for New Setup

- [ ] Installed recommended extensions?
- [ ] Configured settings.json?
- [ ] Created workspace settings?
- [ ] Set up debug configuration?
- [ ] Created tasks.json?
- [ ] Cloned repository?
- [ ] Checked out feature branch?
- [ ] Read AGENTS.md?
- [ ] Read ARCHITECTURE.md?
- [ ] Ready to start coding?

---

**Version**: 1.0  
**Last Updated**: June 2026  
**For**: Cursor IDE Users  
**Project**: MissionBoard EdTech Platform

For general project context, see [AGENTS.md](AGENTS.md). For AI-specific guidelines, see [CLAUDE_README.md](CLAUDE_README.md).
