# MissionBoard - EdTech Platform Documentation

Welcome to MissionBoard, a modern education and learning platform built with cutting-edge cloud technologies.

## 📚 Documentation Index

### Getting Started
- **[README.md](README.md)** - Main project overview and quick start
- **[ARCHITECTURE.md](ARCHITECTURE.md)** - System architecture and technology stack

### Setup Guides
- **[Backend Setup](backend/README.md)** - .NET 8 Appwrite Functions setup
- **[Frontend Setup](frontend/README.md)** - React application setup
- **[Infrastructure Setup](infrastructure/)** - Complete infrastructure configuration

### Infrastructure Configuration
- **[Appwrite Configuration](infrastructure/APPWRITE_CONFIG.md)** - Authentication, OAuth, Functions, and Storage setup
- **[Supabase Configuration](infrastructure/SUPABASE_CONFIG.md)** - Database, migrations, and RLS policies
- **[Database Schema](infrastructure/DATABASE_SCHEMA.md)** - Complete database structure and relationships

### AI Assistant Context
- **[Agents Context](docs/AGENTS.md)** - Context and guidelines for AI agents (Copilot, Claude, Cursor)
- **[Claude Context](docs/CLAUDE_README.md)** - Claude-specific context and best practices
- **[Cursor Context](docs/CURSOR_README.md)** - Cursor IDE setup and context

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm/yarn
- .NET 8 SDK
- Appwrite account (https://appwrite.io)
- Supabase account (https://supabase.com)
- Google OAuth credentials

### Step-by-Step Setup

1. **Clone and Setup**
   ```bash
   git clone https://github.com/ravishankar0267/missionBoard.git
   cd missionBoard
   git checkout feature/edtech-platform-bootstrap
   ```

2. **Frontend Setup** - See [Frontend Setup Guide](frontend/README.md)
   ```bash
   cd frontend
   npm install
   cp .env.example .env.local
   npm start
   ```

3. **Backend Setup** - See [Backend Setup Guide](backend/README.md)
   ```bash
   cd backend
   dotnet restore
   dotnet ef database update
   dotnet run
   ```

4. **Infrastructure Setup** - See [Infrastructure Guides](infrastructure/)
   - [Appwrite Setup](infrastructure/APPWRITE_CONFIG.md)
   - [Supabase Setup](infrastructure/SUPABASE_CONFIG.md)
   - [Database Schema](infrastructure/DATABASE_SCHEMA.md)

## 🏗️ Project Structure

```
missionBoard/
├── backend/                          # .NET 8 Appwrite Functions
│   ├── Functions/                    # Serverless functions
│   ├── Models/                       # Data models
│   ├── Services/                     # Business logic
│   ├── README.md                     # Backend setup
│   └── missionboard.csproj
│
├── frontend/                         # React Application
│   ├── src/
│   │   ├── components/              # React components
│   │   ├── pages/                   # Page components
│   │   ├── hooks/                   # Custom hooks
│   │   ├── context/                 # React Context
│   │   ├── services/                # API services
│   │   └── utils/                   # Utilities
│   ├── README.md                     # Frontend setup
│   └── package.json
│
├── infrastructure/                   # Infrastructure configuration
│   ├── DATABASE_SCHEMA.md            # PostgreSQL schema
│   ├── APPWRITE_CONFIG.md            # Appwrite setup
│   └── SUPABASE_CONFIG.md            # Supabase setup
│
├── docs/                             # Documentation
│   ├── AGENTS.md                     # AI Agent context
│   ├── CLAUDE_README.md              # Claude IDE context
│   ├── CURSOR_README.md              # Cursor IDE context
│   └── INDEX.md                      # This file
│
├── ARCHITECTURE.md                   # System architecture
├── README.md                         # Main README
└── .gitignore
```

## 💡 Tech Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| Frontend | React | 18.2+ |
| Frontend Language | TypeScript | 5.2+ |
| Frontend Styling | Tailwind CSS | 3.3+ |
| Frontend State | React Query | 3.39+ |
| Backend | .NET | 8.0 |
| Backend Language | C# | Latest |
| Backend Hosting | Appwrite Functions | Latest |
| Database | PostgreSQL | 15+ |
| Database Host | Supabase | - |
| Authentication | Appwrite Auth + Google OAuth | - |
| Storage | Appwrite Storage | Latest |
| Hosting | Appwrite Sites | Latest |

## 🔐 Key Features

- ✅ **Google OAuth Authentication** - Secure user authentication
- ✅ **Course Management** - Create and manage courses
- ✅ **Progress Tracking** - Monitor student learning progress
- ✅ **Certificate Generation** - Automated certificate creation
- ✅ **Resource Management** - Store PDFs, videos, and media
- ✅ **Responsive Design** - Works on all devices
- ✅ **Scalable Architecture** - Serverless and cloud-native

## 📖 For AI Agents & Developers

### New to the Project?
Start here based on your role:

- **AI Coding Agents** (Copilot, Claude, Cursor)
  - Read: [AGENTS.md](docs/AGENTS.md) for project context
  - Read: [ARCHITECTURE.md](ARCHITECTURE.md) for system design
  - Refer to: Component-specific READMEs while implementing

- **Backend Developers**
  - Start: [Backend Setup](backend/README.md)
  - Then: [Appwrite Configuration](infrastructure/APPWRITE_CONFIG.md)
  - Then: [Database Schema](infrastructure/DATABASE_SCHEMA.md)

- **Frontend Developers**
  - Start: [Frontend Setup](frontend/README.md)
  - Then: [Architecture Overview](ARCHITECTURE.md)
  - Then: Component structure in `frontend/src/`

- **DevOps/Infrastructure**
  - Start: [ARCHITECTURE.md](ARCHITECTURE.md)
  - Then: [Appwrite Configuration](infrastructure/APPWRITE_CONFIG.md)
  - Then: [Supabase Configuration](infrastructure/SUPABASE_CONFIG.md)

## 🤖 AI Assistant Recommendations

### For GitHub Copilot
Use context from [AGENTS.md](docs/AGENTS.md) to understand the project structure and coding standards.

### For Claude
Review [CLAUDE_README.md](docs/CLAUDE_README.md) for Claude-specific context and best practices for this project.

### For Cursor IDE
Check [CURSOR_README.md](docs/CURSOR_README.md) for Cursor IDE setup, extensions, and project shortcuts.

## 🔗 Documentation Relationships

```
README (This File)
├── ARCHITECTURE.md (System Design)
│   ├── infrastructure/DATABASE_SCHEMA.md
│   ├── infrastructure/APPWRITE_CONFIG.md
│   └── infrastructure/SUPABASE_CONFIG.md
│
├── Backend Development
│   ├── backend/README.md
│   └── infrastructure/APPWRITE_CONFIG.md
│
├── Frontend Development
│   ├── frontend/README.md
│   └── ARCHITECTURE.md
│
└── AI Agent Guides
    ├── docs/AGENTS.md
    ├── docs/CLAUDE_README.md
    └── docs/CURSOR_README.md
```

## 🚀 Development Workflow

1. **Feature Development**
   - Create a feature branch from `feature/edtech-platform-bootstrap`
   - Follow the guidelines in [AGENTS.md](docs/AGENTS.md)
   - Reference [ARCHITECTURE.md](ARCHITECTURE.md) for design patterns

2. **Database Changes**
   - Refer to [Database Schema](infrastructure/DATABASE_SCHEMA.md)
   - Use Supabase CLI for migrations
   - See [Supabase Configuration](infrastructure/SUPABASE_CONFIG.md)

3. **Deployment**
   - Follow [Appwrite Configuration](infrastructure/APPWRITE_CONFIG.md)
   - Follow [Supabase Configuration](infrastructure/SUPABASE_CONFIG.md)
   - Review [ARCHITECTURE.md](ARCHITECTURE.md) deployment pipeline

## 📝 Contributing

When contributing to MissionBoard:

1. Review the [ARCHITECTURE.md](ARCHITECTURE.md) to understand the system
2. For AI Agents: Read [AGENTS.md](docs/AGENTS.md)
3. For specific IDE: Read corresponding context file
4. Follow setup guides for your component
5. Reference infrastructure docs for deployment

## 🆘 Documentation Map

| Need Help With | Read This |
|---|---|
| Understanding the system | [ARCHITECTURE.md](ARCHITECTURE.md) |
| Setting up backend | [Backend Setup](backend/README.md) |
| Setting up frontend | [Frontend Setup](frontend/README.md) |
| Database questions | [Database Schema](infrastructure/DATABASE_SCHEMA.md) |
| Appwrite setup | [Appwrite Config](infrastructure/APPWRITE_CONFIG.md) |
| Supabase setup | [Supabase Config](infrastructure/SUPABASE_CONFIG.md) |
| AI Agent context | [AGENTS.md](docs/AGENTS.md) |
| Claude IDE context | [CLAUDE_README.md](docs/CLAUDE_README.md) |
| Cursor IDE context | [CURSOR_README.md](docs/CURSOR_README.md) |

## 📞 Support Resources

- **Appwrite Documentation**: https://appwrite.io/docs
- **Supabase Documentation**: https://supabase.com/docs
- **React Documentation**: https://react.dev
- **.NET Documentation**: https://learn.microsoft.com/dotnet/

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🎯 Next Steps

1. Choose your role (Backend/Frontend/DevOps/AI Agent)
2. Read the corresponding setup guide
3. Clone the repository and checkout the feature branch
4. Follow the setup instructions
5. Start contributing!

---

**Last Updated**: June 2026  
**Branch**: `feature/edtech-platform-bootstrap`  
**Status**: Documentation Complete - Ready for Development
