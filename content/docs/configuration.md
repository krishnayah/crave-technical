# Configuration & Secrets

## iOS Configuration

### Secrets.swift

Located at `Crave/Secrets.swift` (not committed to Git).

```swift
struct Secrets {
    static let geminiApiKey = "AIza..."
    static let backendUrl = "https://crave-backend-....run.app"
}
```

**Security notes:**
- Add `Secrets.swift` to `.gitignore`
- Never hardcode production API keys
- Consider using Xcode build configurations for multiple environments

### GoogleService-Info.plist

Firebase configuration file. Contains:
- Project ID
- Client ID
- API keys
- Storage bucket

**Important:**
- Download from Firebase Console
- Place in Xcode project root
- Add to `.gitignore` if using multiple environments

### Info.plist

App metadata and configuration:

```xml
<key>CFBundleDisplayName</key>
<string>Crave</string>

<key>UILaunchScreen</key>
<dict>
    <key>UIImageName</key>
    <string>LaunchImage</string>
</dict>

<key>NSCameraUsageDescription</key>
<string>Crave needs camera access to scan ingredients</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>Crave needs photo access to import recipe images</string>
```

### AppStorage Keys

Usage tracking persisted locally:

| Key | Type | Description |
|-----|------|-------------|
| `currentImports` | `Int` | Monthly import count |
| `currentScans` | `Int` | Monthly scan count |
| `lastResetDate` | `Date` | Last monthly reset timestamp |
| `hasCompletedOnboarding` | `Bool` | Whether user has seen onboarding |

## Backend Configuration

### Environment Variables

Set in Cloud Run or `.env` for local development:

| Variable | Description | Required |
|----------|-------------|----------|
| `GEMINI_API_KEY` | Google Gemini API key | Yes |
| `REVENUECAT_V2_API_KEY` | RevenueCat server-side V2 API key | Yes |
| `REVENUECAT_PROJECT_ID` | RevenueCat project identifier | Yes |
| `PORT` | Server port (default: 8080) | No |

### .env File (Local Development)

```env
GEMINI_API_KEY=your_gemini_key_here
REVENUECAT_V2_API_KEY=your_revenuecat_key_here
REVENUECAT_PROJECT_ID=your_project_id_here
PORT=8000
```

**Never commit this file!**

### config.py

Centralized settings loader:

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    gemini_api_key: str
    revenuecat_v2_api_key: str
    revenuecat_project_id: str
    port: int = 8080

    class Config:
        env_file = ".env"

settings = Settings()
```

## Firebase Configuration

### Project Setup

1. Create Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Add iOS app with bundle ID: `com.yourcompany.crave`
3. Download `GoogleService-Info.plist`
4. Enable Authentication → Sign-in methods → Google
5. Enable Firestore Database
6. Enable Storage

### Firestore Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // User can only read/write their own data
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    // Public recipes are readable by all, writable only by owner
    match /public/recipes/{recipeId} {
      allow read: if true;
      allow write: if request.auth != null && request.resource.data.authorId == request.auth.uid;
    }
  }
}
```

### Storage Rules

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /users/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## RevenueCat Configuration

### Project Setup

1. Create project at [app.revenuecat.com](https://app.revenuecat.com)
2. Add iOS app
3. Configure App Store Connect integration
4. Create entitlement: "Crave Pro"
5. Create offering with subscription products
6. Copy API keys

### API Keys

| Key Type | Usage | Location |
|----------|-------|----------|
| Public (iOS SDK) | Client-side purchases | `CraveApp.swift` |
| Secret (V2 API) | Server-side operations | Backend `.env` |

### Entitlements

```
Crave Pro
  - Monthly: $4.99/month
  - Annual: $39.99/year (save 33%)
```

### Subscription Products

Create in App Store Connect, then link in RevenueCat:
- `crave_pro_monthly`
- `crave_pro_annual`

## Multi-Environment Setup

### iOS Build Configurations

1. Duplicate "Debug" and "Release" configurations
2. Create "Staging" configuration
3. Use build settings to switch API URLs

**Example:**

```swift
#if STAGING
static let backendUrl = "https://crave-backend-staging.run.app"
#elseif DEBUG
static let backendUrl = "http://localhost:8000"
#else
static let backendUrl = "https://crave-backend-prod.run.app"
#endif
```

### Backend Environments

Deploy separate Cloud Run services:
- `crave-backend-dev`
- `crave-backend-staging`
- `crave-backend-prod`

Use different environment variables for each.

## Security Best Practices

1. **Never commit secrets** — Use `.gitignore` for all sensitive files
2. **Rotate keys regularly** — Update API keys every 90 days
3. **Use environment variables** — Never hardcode production credentials
4. **Validate on backend** — Don't trust client-side data
5. **Enable HTTPS only** — Reject insecure connections
6. **Monitor usage** — Set up alerts for unusual API activity
7. **Least privilege** — Grant minimum necessary permissions

## Troubleshooting

### "Firebase not initialized"

- Ensure `GoogleService-Info.plist` is in Xcode project
- Verify `Firebase.configure()` is called in `CraveApp.init()`

### "RevenueCat API key invalid"

- Check key format (should start with `pk_` for public, `sk_` for secret)
- Verify key matches the platform (iOS vs Android)
- Ensure key hasn't been revoked in dashboard

### "Gemini API quota exceeded"

- Check quota limits in Google Cloud Console
- Enable billing for higher quotas
- Implement caching to reduce API calls

### "Backend unreachable"

- Verify Cloud Run service is running
- Check Cloud Run logs for errors
- Ensure environment variables are set correctly
- Validate network connectivity from iOS device
