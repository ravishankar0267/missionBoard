# SENTRY_SETUP.md - Error Logging & Crash Reporting

This document provides comprehensive setup instructions for integrating Sentry error logging and crash reporting across all MissionBoard platforms.

## 🎯 Overview

Sentry is integrated across:
- **Frontend (React)** - Browser errors and performance
- **Backend (.NET)** - Function errors and exceptions
- **Mobile (Kotlin Multiplatform)** - Android and iOS crashes
- **Database** - Query performance monitoring

## 📊 What Sentry Tracks

### ✅ Tracked Events
- Uncaught exceptions
- JavaScript errors (frontend)
- Unhandled runtime errors (backend)
- Crash reports (mobile)
- Performance issues
- Missing resources
- API failures
- Database query performance
- Release deployments
- Custom events

### 📈 Key Metrics
- Error frequency and patterns
- Affected users count
- Release health
- Performance percentiles
- Crash-free sessions

## 🔑 Setup Steps

### Step 1: Create Sentry Project

1. Sign up at https://sentry.io
2. Create new organization (if needed)
3. Create projects for each platform:
   - **Frontend** (JavaScript/React)
   - **Backend** (.NET)
   - **Mobile Android** (Kotlin)
   - **Mobile iOS** (Swift)

4. Get DSN (Data Source Name) for each project
   - Format: `https://<key>@<org>.ingest.sentry.io/<projectId>`

### Step 2: Frontend Setup (React)

#### Installation

```bash
cd frontend
npm install @sentry/react @sentry/tracing
```

#### Configuration (main.tsx or index.tsx)

```typescript
import * as Sentry from "@sentry/react";
import { BrowserTracing } from "@sentry/tracing";

Sentry.init({
  dsn: process.env.REACT_APP_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  release: process.env.REACT_APP_VERSION,
  
  // Performance Monitoring
  tracesSampleRate: process.env.NODE_ENV === "production" ? 0.1 : 1.0,
  integrations: [
    new BrowserTracing(),
    new Sentry.Replay({
      maskAllText: true,
      blockAllMedia: true,
    }),
  ],
  
  // Session Replay
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
  
  // Ignore certain errors
  ignoreErrors: [
    // Browser extensions
    "top.GLOBALS",
    // Random plugins/extensions
    "chrome-extension://",
    "moz-extension://",
  ],
});

// Wrap root component
const App = Sentry.withProfiler(() => {
  return <YourApp />;
});

export default App;
```

#### Environment Variables (.env.local)

```
REACT_APP_SENTRY_DSN=https://<key>@<org>.ingest.sentry.io/<projectId>
REACT_APP_VERSION=1.0.0
```

#### Usage in React

```typescript
// Automatic error capture
try {
  riskyOperation();
} catch (error) {
  Sentry.captureException(error);
}

// Custom message
Sentry.captureMessage("Something went wrong", "error");

// Set user context
Sentry.setUser({
  id: userId,
  email: userEmail,
  username: userName,
});

// Set custom context
Sentry.setContext("character", {
  name: "Mighty Fighter",
  level: 19,
});

// Add breadcrumb for debugging
Sentry.addBreadcrumb({
  category: "auth",
  message: "User logged in",
  level: "info",
});
```

### Step 3: Backend Setup (.NET)

#### Installation

```bash
cd backend
dotnet add package Sentry.AspNetCore --version 7.0.0
```

#### Configuration (Program.cs)

```csharp
using Sentry.AspNetCore;

var builder = WebApplication.CreateBuilder(args);

// Add Sentry
builder.WebHost.UseSentry(options =>
{
    options.Dsn = builder.Configuration["Sentry:Dsn"];
    options.Environment = builder.Environment.EnvironmentName;
    options.Release = "1.0.0";
    
    // Performance monitoring
    options.TracesSampleRate = builder.Environment.IsProduction() ? 0.1 : 1.0;
    
    // Enable debug mode in development
    options.Debug = builder.Environment.IsDevelopment();
    
    // Ignore certain exceptions
    options.AddEventProcessor((evt, hint) =>
    {
        if (hint.OriginalException is OperationCanceledException)
            return null;
        return evt;
    });
});

// ... rest of configuration
var app = builder.Build();

// ... middleware setup
app.UseSentryTracing();
app.Run();
```

#### appsettings.json

```json
{
  "Sentry": {
    "Dsn": "https://<key>@<org>.ingest.sentry.io/<projectId>",
    "Environment": "Development",
    "TracesSampleRate": 1.0,
    "ProfilesSampleRate": 1.0
  }
}
```

#### Usage in .NET Functions

```csharp
using Sentry;

[Function]
public async Task<dynamic> MyFunction(string userId)
{
    using var transaction = SentrySdk.StartTransaction("my-function", "task");
    
    try
    {
        // Business logic
        var span = transaction.StartChild("database-query");
        var user = await dbContext.Users.FindAsync(userId);
        span.Finish();
        
        return new { success = true, data = user };
    }
    catch (Exception ex)
    {
        // Capture exception with context
        SentrySdk.CaptureException(ex);
        transaction.Status = SpanStatus.InternalError;
        
        return new { success = false, error = ex.Message };
    }
    finally
    {
        transaction.Finish();
    }
}

// Set user context
SentrySdk.ConfigureScope(scope =>
{
    scope.SetUser(new User
    {
        Id = userId,
        Email = userEmail,
        Username = userName
    });
});

// Capture custom event
SentrySdk.CaptureMessage("Course enrolled successfully", SentryLevel.Info);
```

### Step 4: Mobile Setup (Kotlin Multiplatform)

#### Android Setup

##### Installation (build.gradle.kts - androidApp)

```kotlin
dependencies {
    implementation("io.sentry:sentry-android:7.0.0")
    implementation("io.sentry:sentry-android-core:7.0.0")
}
```

##### Configuration (AndroidManifest.xml)

```xml
<manifest>
    <uses-permission android:name="android.permission.INTERNET" />
    
    <application>
        <meta-data
            android:name="io.sentry.dsn"
            android:value="https://<key>@<org>.ingest.sentry.io/<projectId>" />
        
        <meta-data
            android:name="io.sentry.environment"
            android:value="production" />
        
        <meta-data
            android:name="io.sentry.traces.sample-rate"
            android:value="0.1" />
        
        <meta-data
            android:name="io.sentry.attach-threads"
            android:value="true" />
    </application>
</manifest>
```

##### Setup in MainActivity.kt

```kotlin
import io.sentry.Sentry
import io.sentry.android.core.SentryAndroid

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Initialize Sentry
        SentryAndroid.init(this) { options ->
            options.dsn = BuildConfig.SENTRY_DSN
            options.environment = BuildConfig.BUILD_TYPE
            options.release = BuildConfig.VERSION_NAME
            options.tracesSampleRate = 0.1
            options.enableUserInteractionBreadcrumbs = true
            options.enableAppLifecycleBreadcrumbs = true
        }
        
        setContent {
            MissionBoardApp()
        }
    }
}
```

##### Usage in Kotlin

```kotlin
// Capture exception
try {
    riskyOperation()
} catch (e: Exception) {
    Sentry.captureException(e)
}

// Set user context
Sentry.setUser(User().apply {
    id = userId
    email = userEmail
    username = userName
})

// Add breadcrumb
Sentry.addBreadcrumb(Breadcrumb().apply {
    category = "auth"
    message = "User logged in"
    level = SentryLevel.INFO
})

// Capture message
Sentry.captureMessage("Course enrolled", SentryLevel.INFO)

// Start transaction
val transaction = Sentry.startTransaction("course-enrollment", "task")
try {
    // Long operation
    enrollInCourse()
} finally {
    transaction.finish()
}
```

#### iOS Setup

##### Installation (Podfile)

```ruby
pod 'Sentry', :git => 'https://github.com/getsentry/sentry-cocoa.git'
```

Then run:
```bash
pod install
```

##### Configuration (SceneDelegate.swift or AppDelegate.swift)

```swift
import Sentry

func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
) -> Bool {
    SentrySDK.start { options in
        options.dsn = "https://<key>@<org>.ingest.sentry.io/<projectId>"
        options.environment = "production"
        options.release = Bundle.main.appVersion
        options.tracesSampleRate = 0.1
        options.enableUserInteractionBreadcrumbs = true
    }
    
    return true
}
```

##### Usage in Swift

```swift
import Sentry

// Capture exception
do {
    try riskyOperation()
} catch {
    SentrySDK.capture(error: error)
}

// Set user
SentrySDK.setUser(User(userId: userId, email: userEmail, username: userName))

// Add breadcrumb
SentrySDK.addBreadcrumb(Breadcrumb(level: .info, category: "auth", message: "User logged in"))

// Capture message
SentrySDK.capture(message: "Course enrolled", level: .info)

// Start transaction
let transaction = SentrySDK.startTransaction(name: "course-enrollment", op: "task")
defer { transaction?.finish() }

enrollInCourse()
```

### Step 5: Shared Mobile Code Setup

#### In commonMain (build.gradle.kts)

```kotlin
commonMain.dependencies {
    // Sentry for multiplatform
    implementation("io.sentry:sentry:7.0.0")
}
```

#### Shared Error Handler

```kotlin
// shared/src/commonMain/kotlin/domain/error/ErrorHandler.kt

expect class ErrorHandler {
    fun captureException(exception: Exception)
    fun captureMessage(message: String)
    fun setUser(userId: String, email: String? = null)
}

// Android implementation
actual class ErrorHandler {
    actual fun captureException(exception: Exception) {
        Sentry.captureException(exception)
    }
    
    actual fun captureMessage(message: String) {
        Sentry.captureMessage(message)
    }
    
    actual fun setUser(userId: String, email: String?) {
        Sentry.setUser(User().apply {
            id = userId
            this.email = email
        })
    }
}

// iOS implementation
actual class ErrorHandler {
    actual fun captureException(exception: Exception) {
        SentrySDK.capture(error: exception)
    }
    
    actual fun captureMessage(message: String) {
        SentrySDK.capture(message: message)
    }
    
    actual fun setUser(userId: String, email: String?) {
        SentrySDK.setUser(User(userId: userId, email: email))
    }
}
```

## 📈 Monitoring & Alerts

### Sentry Dashboard

1. **Issues** - View all issues grouped by error type
2. **Releases** - Track errors per release
3. **Performance** - Monitor slow operations
4. **Users** - See affected users
5. **Discover** - Custom queries and analytics

### Setting Up Alerts

1. **Alert Rules** → **New Alert Rule**
2. Configure:
   - **Filter**: `is:unresolved` (new errors)
   - **Environment**: `production`
   - **Threshold**: 10 errors in 5 minutes
   - **Action**: Send notification to Slack/Email

### Example Alert Conditions

```
When: An issue is first seen
Then: Send notification to #alerts-critical

When: Number of errors > 100 in 1 hour
Then: Send notification to #alerts-production

When: Error rate > 5% 
Then: Create PagerDuty incident
```

## 🔗 Release Tracking

### Release Management

```bash
# Create release
sentry-cli releases -o org-name -p project-name create "1.0.0"

# Associate commits
sentry-cli releases -o org-name -p project-name set-commits "1.0.0" --auto

# Finalize release
sentry-cli releases -o org-name -p project-name finalize "1.0.0"
```

### Release Notes in Sentry

Configure in Sentry UI:
1. Project Settings → Release Tracking
2. Add version to frontend/package.json
3. Add version to backend properties
4. Include in release notes

## 🔍 Source Maps

### Frontend Source Maps

```bash
# Generate source maps
npm run build

# Upload to Sentry
sentry-cli releases -o org-name -p project-name files upload-sourcemaps ./build/static/js
```

### Backend Source Maps

Source maps are handled automatically for .NET applications.

## 🛡️ Security & Privacy

### Personally Identifiable Information (PII)

```typescript
// Frontend - Mask sensitive data
Sentry.init({
  beforeSend(event) {
    // Remove email
    if (event.request) {
      event.request.cookies = undefined;
    }
    return event;
  },
});
```

### Data Retention

- Free plan: 90 days
- Paid plan: Configurable retention
- GDPR compliance: Built-in

### Environment Variables

Never commit sensitive data:

```bash
# .env (NOT committed)
REACT_APP_SENTRY_DSN=https://...
SENTRY_DSN=https://...
```

## 📝 Best Practices

### Do's ✅
- Set user context for all errors
- Use breadcrumbs to track user actions
- Track performance-critical operations
- Use release tracking for better debugging
- Configure appropriate sample rates
- Review errors regularly

### Don'ts ❌
- Don't commit DSNs to repository
- Don't expose sensitive user data
- Don't track PII without consent
- Don't spam with custom events
- Don't ignore errors in production
- Don't use production DSN in development

## 🔗 Integration Examples

### API Error Handling

```typescript
// Frontend
async function enrollCourse(courseId: string) {
  const transaction = Sentry.startTransaction({
    name: "Course Enrollment",
    op: "enrollment",
  });

  try {
    const response = await api.post("/enroll", { courseId });
    transaction.setStatus("ok");
    return response;
  } catch (error) {
    Sentry.captureException(error);
    transaction.setStatus("error");
    throw error;
  } finally {
    transaction.finish();
  }
}
```

### Authentication Error Tracking

```csharp
// Backend
[Function]
public async Task<dynamic> Login(string email, string password)
{
    try
    {
        var user = await AuthService.LoginAsync(email, password);
        SentrySdk.SetUser(new User { Id = user.Id, Email = email });
        return new { success = true, user };
    }
    catch (InvalidOperationException ex)
    {
        SentrySdk.CaptureMessage($"Login failed: {email}", SentryLevel.Warning);
        return new { success = false, error = "Invalid credentials" };
    }
}
```

### Database Query Monitoring

```kotlin
// Mobile
suspend fun getCourses(): List<Course> {
    val span = Sentry.startTransaction("get-courses", "database")
    return try {
        courseRepository.getCourses().also {
            span?.setStatus(SpanStatus.Ok)
        }
    } catch (e: Exception) {
        Sentry.captureException(e)
        span?.setStatus(SpanStatus.InternalError)
        emptyList()
    } finally {
        span?.finish()
    }
}
```

## 📊 Metrics Dashboard

Create custom dashboards in Sentry:

```
1. Errors by Platform
2. Error Trends (7-day)
3. Most Affected Users
4. Performance P95 Latency
5. Crash-Free Sessions Rate
6. API Response Times
```

## 🆘 Troubleshooting

### Issues Not Appearing

- Check DSN is correct
- Verify environment name matches
- Check sample rates (might be filtering)
- Ensure network requests aren't blocked

### Performance Issues

- Reduce `tracesSampleRate` (currently 0.1 for production)
- Disable replay for high-volume events
- Use event filters to ignore noise

### Missing Context

- Ensure `setUser()` is called after authentication
- Add breadcrumbs at key application points
- Include custom context in error messages

## 📞 Support

- **Sentry Docs**: https://docs.sentry.io/
- **Sentry Status**: https://status.sentry.io/
- **Community Forum**: https://forum.sentry.io/

## 🔗 Related Documentation

- [AGENTS.md](docs/AGENTS.md) - General project guidelines
- [ARCHITECTURE.md](ARCHITECTURE.md) - System design
- [app/README.md](app/README.md) - Mobile app structure
- [backend/README.md](backend/README.md) - Backend setup
- [frontend/README.md](frontend/README.md) - Frontend setup

---

**Version**: 1.0  
**Last Updated**: June 2026  
**Maintained By**: DevOps Team

For questions about error logging, refer to Sentry documentation or reach out to the platform team.
