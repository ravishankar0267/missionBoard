# Appwrite Configuration

## Project Setup

### Create API Keys

1. Go to Appwrite Console → Settings → API Keys
2. Create the following keys:
   - **Frontend API Key**: Scope = auth.read, users.read, storage.read
   - **Backend API Key**: Scope = Full admin access

### Configure OAuth (Google)

1. Create Google OAuth credentials at https://console.cloud.google.com
2. In Appwrite Console → Settings → OAuth 2.0 Providers
3. Configure Google:
   - Client ID: Your Google Client ID
   - Client Secret: Your Google Client Secret
   - Redirect URI: `https://your-appwrite-endpoint/auth/callback`

### Create Collections

Create the following Appwrite collections for any frontend caching/syncing:

#### User Preferences
```
Collection: user_preferences
Attributes:
- user_id (String) - Indexed
- theme (String) - Default: 'light'
- notifications_enabled (Boolean) - Default: true
- language (String) - Default: 'en'
```

### Create Buckets

#### Course Resources Bucket
```
Bucket: course-resources
File Permissions: Public
Maximum Size: 1GB per file
```

#### User Uploads Bucket
```
Bucket: user-uploads
File Permissions: Private (signed URLs)
Maximum Size: 500MB per file
```

#### Certificates Bucket
```
Bucket: certificates
File Permissions: Private
Maximum Size: 50MB per file
```

## Functions Deployment

### Deploy via Appwrite CLI

```bash
# Login
appwrite login

# Deploy all functions
appwrite deploy function

# Deploy specific function
appwrite deploy function --functionId <function-id>
```

### Function Configuration

Example function configuration (appwrite.json):

```json
{
  "functions": [
    {
      "$id": "user-onboarding",
      "name": "User Onboarding",
      "runtime": "dotnet-8.0",
      "path": "./backend/Functions/Auth/UserOnboarding",
      "entrypoint": "UserOnboarding.cs",
      "execute": ["any"],
      "env": [
        {
          "key": "SUPABASE_URL",
          "value": "${SUPABASE_URL}"
        },
        {
          "key": "SUPABASE_KEY",
          "value": "${SUPABASE_KEY}"
        }
      ]
    }
  ]
}
```

## Environment Variables

Set these in Appwrite Console → Settings → Environment Variables:

```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-service-role-key
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
APP_NAME=MissionBoard
APP_URL=https://your-app-url
```

## Security

### API Key Protection
- Store API keys in environment variables
- Never commit keys to version control
- Rotate keys periodically
- Use different keys for different environments

### Function Permissions
- Set minimum required scopes
- Use JWT tokens for function-to-function calls
- Validate function execution context

### File Security
- Use signed URLs for private file access
- Set expiration time on signed URLs
- Implement custom validation for file uploads
- Scan uploaded files for malware

## Monitoring

### Function Logs
View function execution logs in Appwrite Console → Functions → Logs

### Error Tracking
- Implement custom error handling
- Log errors to external service (Sentry, etc.)
- Set up alerts for critical errors

### Performance Monitoring
- Monitor function execution time
- Track memory usage
- Analyze cold start times

## Backup

### Code Backup
```bash
# Export all functions
appwrite export
```

### Storage Backup
Configure daily backups in Appwrite Console → Settings → Backups
