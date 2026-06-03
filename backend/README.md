# Backend - EdTech Platform Functions

## Tech Stack
- **.NET 8**: Latest C# features and performance
- **Appwrite Functions**: Serverless execution
- **Supabase PostgreSQL**: Data persistence
- **Entity Framework Core**: ORM for database access

## Project Structure

```
backend/
├── Functions/
│  ├── Auth/
│  │  ├── UserOnboarding.cs
│  │  ├── ProfileManagement.cs
│  │  └── GoogleOAuth.cs
│  ├── Courses/
│  │  ├── CourseCreate.cs
│  │  ├── CourseUpdate.cs
│  │  ├── CourseDelete.cs
│  │  ├── CourseList.cs
│  │  └── CourseEnroll.cs
│  ├── Progress/
│  │  ├── UpdateProgress.cs
│  │  ├── GetProgress.cs
│  │  └── GenerateCertificate.cs
│  └── Notifications/
│     ├── SendEmail.cs
│     └── SendNotification.cs
├── Models/
│  ├── User.cs
│  ├── Course.cs
│  ├── Enrollment.cs
│  ├── Progress.cs
│  └── Certificate.cs
├── Data/
│  └── AppDbContext.cs
├── Services/
│  ├── AuthService.cs
│  ├── CourseService.cs
│  ├── ProgressService.cs
│  └── NotificationService.cs
├── Program.cs
├── appsettings.json
└── missionboard.csproj
```

## Setup

### Prerequisites
- .NET 8 SDK installed
- Supabase project created
- Appwrite Functions enabled

### Configuration

1. Create `appsettings.json`:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=your-supabase-host;Database=missionboard;Username=postgres;Password=your-password"
  },
  "Appwrite": {
    "Endpoint": "https://your-appwrite-endpoint",
    "ProjectId": "your-project-id",
    "ApiKey": "your-api-key"
  }
}
```

2. Install dependencies:
```bash
dotnet restore
```

3. Run migrations:
```bash
dotnet ef database update
```

## Development

```bash
dotnet run
```

## Deployment

### Deploy to Appwrite Functions

1. Initialize Appwrite CLI:
```bash
appwrite login
```

2. Deploy functions:
```bash
appwrite deploy function
```

## Key Functions

### Authentication
- **UserOnboarding**: Create user profile on first login
- **GoogleOAuth**: Handle Google OAuth flow
- **ProfileManagement**: Update user profile

### Course Management
- **CourseCreate**: Create new courses
- **CourseList**: Fetch available courses
- **CourseEnroll**: Enroll user in course

### Progress Tracking
- **UpdateProgress**: Log learning progress
- **GenerateCertificate**: Generate completion certificate

### Notifications
- **SendEmail**: Send email notifications
- **SendNotification**: In-app notifications

## Testing

```bash
dotnet test
```

## Contributing

See CONTRIBUTING.md for guidelines.
