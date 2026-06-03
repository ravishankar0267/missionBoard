# Frontend - EdTech Platform React App

## Tech Stack
- **React 18**: Latest React features
- **TypeScript**: Type safety
- **Appwrite SDK**: Backend integration
- **Supabase JS**: Database client
- **React Query**: Data fetching and state management
- **Tailwind CSS**: Styling
- **React Router**: Navigation

## Project Structure

```
frontend/
├── public/
├── src/
│  ├── components/
│  │  ├── Auth/
│  │  │  ├── LoginForm.tsx
│  │  │  ├── GoogleSignIn.tsx
│  │  │  └── ProtectedRoute.tsx
│  │  ├── Dashboard/
│  │  │  ├── Dashboard.tsx
│  │  │  ├── UserProfile.tsx
│  │  │  └── ProgressCard.tsx
│  │  ├── Courses/
│  │  │  ├── CourseList.tsx
│  │  │  ├── CourseCard.tsx
│  │  │  ├── CourseDetail.tsx
│  │  │  ├── CourseEnroll.tsx
│  │  │  └── ResourceViewer.tsx
│  │  ├── Learning/
│  │  │  ├── VideoPlayer.tsx
│  │  │  ├── PDFViewer.tsx
│  │  │  └── LessonCompletion.tsx
│  │  └── Common/
│  │     ├── Header.tsx
│  │     ├── Sidebar.tsx
│  │     └── Footer.tsx
│  ├── pages/
│  │  ├── LoginPage.tsx
│  │  ├── DashboardPage.tsx
│  │  ├── CoursesPage.tsx
│  │  ├── LearningPage.tsx
│  │  └── NotFoundPage.tsx
│  ├── hooks/
│  │  ├── useAuth.ts
│  │  ├── useCourses.ts
│  │  ├── useProgress.ts
│  │  └── useStorage.ts
│  ├── context/
│  │  ├── AuthContext.tsx
│  │  ├── ThemeContext.tsx
│  │  └── NotificationContext.tsx
│  ├── services/
│  │  ├── appwrite.ts
│  │  ├── supabase.ts
│  │  ├── api.ts
│  │  └── storage.ts
│  ├── utils/
│  │  ├── constants.ts
│  │  ├── formatters.ts
│  │  └── validators.ts
│  ├── styles/
│  │  ├── tailwind.css
│  │  └── globals.css
│  ├── App.tsx
│  └── index.tsx
├── .env.example
├── package.json
├── tsconfig.json
├── tailwind.config.js
└── README.md
```

## Setup

### Prerequisites
- Node.js 18+ and npm/yarn
- Appwrite instance running
- Supabase project created

### Installation

1. Install dependencies:
```bash
npm install
```

2. Create `.env.local` file:
```
REACT_APP_APPWRITE_ENDPOINT=https://your-appwrite-endpoint
REACT_APP_APPWRITE_PROJECT_ID=your-project-id
REACT_APP_SUPABASE_URL=https://your-project.supabase.co
REACT_APP_SUPABASE_ANON_KEY=your-anon-key
```

## Development

```bash
npm start
```

Open http://localhost:3000 to view it in the browser.

## Build

```bash
npm run build
```

Builds the app for production to the `build` folder.

## Deployment to Appwrite Sites

1. Build the app:
```bash
npm run build
```

2. Deploy to Appwrite Sites:
```bash
appwrite deploy site
```

## Key Features

### Authentication
- Google OAuth 2.0 login
- Secure session management
- Protected routes

### Dashboard
- User profile management
- Learning progress overview
- Recent courses
- Personalized recommendations

### Course Management
- Browse available courses
- Course details and syllabus
- Enrollment system
- Progress tracking

### Learning Experience
- Video player with playback controls
- PDF viewer for documents
- Lesson completion tracking
- Certificate generation

### File Management
- Upload course materials
- Secure file access with signed URLs
- Progress synchronization

## Testing

```bash
npm test
```

## Code Quality

```bash
npm run lint
```

## Contributing

See CONTRIBUTING.md for guidelines.
