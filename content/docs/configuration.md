# Configuration & Secrets

## iOS Configuration

### Secrets.swift

Located at `Crave/Secrets.swift` (not committed to Git):

```swift
struct Secrets {
    static let geminiApiKey = "AIza..."
    static let backendUrl = "https://crave-backend-....run.app"
}
```

### Config.swift

Centralized constants at `Crave/Config.swift`:

```swift
enum Config {
    static let freeImportLimit = 5
    static let freeScanLimit = 10
    static let imageMaxSide: CGFloat = 1536
    static let imageCompressionQuality: CGFloat = 0.8
    static let maxDetectedItems = 50
}
```

Also contains AppStorage key helpers and a per-user onboarding key function.

### AppStorage Keys

| Key | Type | Description |
|-----|------|-------------|
| `monthlyImportsCount` | `Int` | Monthly import count |
| `monthlyScansCount` | `Int` | Monthly scan count |
| `lastResetDate` | `Double` | Last monthly reset (TimeInterval) |
| `hasSeenOnboarding_{userId}` | `Bool` | Per-user onboarding flag |
| `developerModeEnabled` | `Bool` | Developer mode toggle |
| `userAllergens` | `[String]` | Selected allergen raw values |

## Backend Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `GEMINI_API_KEY` | Google Gemini API key | Yes |
| `REVENUECAT_V2_API_KEY` | RevenueCat server-side V2 API key | Yes |
| `REVENUECAT_PROJECT_ID` | RevenueCat project identifier | Yes |
| `YOUTUBE_COOKIES_B64` | Base64-encoded cookies.txt for yt-dlp | No |
| `PORT` | Server port (default: 8080) | No |

## Firebase Configuration

### Firestore Collections

| Collection | Description |
|------------|-------------|
| `users/{uid}/recipes` | User's private recipes |
| `users/{uid}/pantry` | User's pantry items |
| `users/{uid}/grocery` | User's grocery items |
| `recipes` | Public explore feed (`isPublic == true`) |

### Storage Paths

Recipe images: `recipes/{userId}/{recipeId}.jpg` (resized to max 1024px, 0.6 JPEG quality).

## RevenueCat

| Key Type | Prefix | Location |
|----------|--------|----------|
| Public (iOS SDK) | `appl_` | `CraveApp.swift` |
| Secret (V2 API) | Bearer token | Backend `.env` |

## Security

1. **Never commit secrets** -- `Secrets.swift`, `.env`, `GoogleService-Info.plist` in `.gitignore`
2. **Per-user data isolation** -- Firestore rules scope to `request.auth.uid`
3. **Stateless backend** -- No session management
4. **Image processing limits** -- Max 1536px for scans, 1024px for uploads
