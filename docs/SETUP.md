# Setup Instructions

## Prerequisites

- Node.js 18+ and npm/yarn
- .NET 8 SDK
- Appwrite Account (https://appwrite.io)
- Supabase Account (https://supabase.com)
- Google OAuth App credentials

## Step 1: Appwrite Project Setup

1. Create an Appwrite project at https://appwrite.io
2. Note your **Project ID** and **API Endpoint**
3. Create a new API key with appropriate scopes
4. Set up Google OAuth:
   - Go to Google Cloud Console
   - Create OAuth 2.0 credentials
   - Add redirect URI: `https://your-appwrite-endpoint/auth/callback`

## Step 2: Supabase Setup

1. Create a Supabase project at https://supabase.com
2. Note your **Project URL** and **API Key**
3. Run database migrations (see `/backend` README)
4. Set up Row Level Security (RLS) policies

## Step 3: Environment Configuration

### Frontend (.env.local)
```
REACT_APP_APPWRITE_ENDPOINT=https://your-appwrite-endpoint
REACT_APP_APPWRITE_PROJECT_ID=your-project-id
REACT_APP_APPWRITE_API_KEY=your-api-key
REACT_APP_SUPABASE_URL=https://your-project.supabase.co
REACT_APP_SUPABASE_ANON_KEY=your-anon-key
```

### Backend (.env)
```
APPWRITE_ENDPOINT=https://your-appwrite-endpoint
APPWRITE_PROJECT_ID=your-project-id
APPWRITE_API_KEY=your-api-key
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-service-role-key
GOOGLE_CLIENT_ID=your-google-client-id
```

## Step 4: Local Development

### Frontend
```bash
cd frontend
npm install
npm start
```

### Backend
```bash
cd backend
dotnet restore
dotnet run
```

## Step 5: Deployment

### Deploy Frontend to Appwrite Sites
```bash
cd frontend
npm run build
# Upload build folder to Appwrite Sites
```

### Deploy Backend Functions
```bash
cd backend
# Deploy functions to Appwrite
appwrite deploy
```
