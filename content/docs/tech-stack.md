# Tech Stack

## iOS App

| Layer | Technology |
|-------|------------|
| **UI Framework** | SwiftUI (iOS 16+) |
| **Architecture** | MVVM with singleton services |
| **Authentication** | Firebase Auth + Google Sign-In |
| **Database** | Cloud Firestore (real-time listeners) |
| **File Storage** | Firebase Storage |
| **Subscriptions** | RevenueCat SDK 5.58+ / RevenueCatUI |
| **AI / Vision** | Google Generative AI (Gemini) on-device |
| **Networking** | URLSession (async/await) |
| **Local Persistence** | `@AppStorage` (UserDefaults) for usage tracking |

## Backend

| Layer | Technology |
|-------|------------|
| **Framework** | FastAPI (Python 3.11) |
| **AI** | Google Generative AI (Gemini) |
| **Video Processing** | yt-dlp + FFmpeg + Deno |
| **Hosting** | Google Cloud Run (Docker) |
| **HTTP** | `requests` library |

## Third-Party SDKs (iOS)

| Package | Purpose |
|---------|---------|
| `firebase-ios-sdk` | Auth, Firestore, Storage |
| `GoogleSignIn-iOS` | Google OAuth |
| `purchases-ios` | RevenueCat subscription management |
| `RevenueCatUI` | Paywall and Customer Center UI |
| `generative-ai-swift` | On-device Gemini Vision |

## Why These Choices?

### SwiftUI
Modern, declarative UI framework that enables rapid iteration and clean, maintainable code.

### Firebase
Real-time sync across devices, robust authentication, and seamless integration with iOS.

### RevenueCat
Takes the complexity out of subscription management with a unified API across platforms.

### Gemini AI
State-of-the-art multimodal AI for both text extraction and vision-based ingredient detection.

### FastAPI
High-performance Python framework with automatic API documentation and async support.

### Cloud Run
Serverless deployment that scales to zero, reducing costs while maintaining performance.
