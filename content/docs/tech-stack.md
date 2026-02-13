# Tech Stack

## iOS App

| Layer | Technology |
|-------|------------|
| **UI Framework** | SwiftUI (iOS 16+) |
| **Architecture** | MVVM with singleton services |
| **Authentication** | Firebase Auth (Email/Password + Google Sign-In) |
| **Database** | Cloud Firestore (real-time listeners, client-side sort) |
| **File Storage** | Firebase Storage (image upload with resize/compression) |
| **Subscriptions** | RevenueCat SDK / RevenueCatUI |
| **AI (Vision)** | Gemini 2.5 Pro via REST API (ingredient scanning) |
| **AI (Text)** | Gemini 2.0 Flash via REST API (generation, tagging, search) |
| **Networking** | URLSession (async/await) with retry + exponential backoff |
| **Image Caching** | Two-tier: NSCache (memory, 50MB) + FileManager (disk, 100MB) |
| **Local Persistence** | `@AppStorage` / UserDefaults for usage tracking |
| **Connectivity** | NWPathMonitor for offline detection |
| **Haptics** | UIImpactFeedbackGenerator / UINotificationFeedbackGenerator |

## Backend

| Layer | Technology |
|-------|------------|
| **Framework** | FastAPI (Python 3.11) |
| **AI** | Gemini 2.0 Flash (`google-generativeai`) |
| **Video Processing** | yt-dlp + FFmpeg + Deno |
| **Hosting** | Google Cloud Run (Docker, Python 3.11-slim) |
| **HTTP** | `requests` library |

## Third-Party SDKs (iOS)

| Package | Purpose |
|---------|---------|
| `firebase-ios-sdk` | Auth, Firestore, Storage |
| `GoogleSignIn-iOS` | Google OAuth |
| `purchases-ios` | RevenueCat subscription management |
| `RevenueCatUI` | Paywall and Customer Center UI |

## Why These Choices?

### SwiftUI
Modern, declarative UI framework that enables rapid iteration and clean, maintainable code.

### Firebase
Real-time sync across devices, robust authentication, and seamless integration with iOS.

### RevenueCat
Takes the complexity out of subscription management with a unified API across platforms.

### Gemini AI (Dual Model Strategy)
- **Gemini 2.5 Pro** for vision tasks -- superior image analysis for ingredient scanning
- **Gemini 2.0 Flash** for text tasks -- fast and cost-effective for generation, tagging, and search

### FastAPI
High-performance Python framework with automatic API documentation and async support.

### Cloud Run
Serverless deployment that scales to zero, reducing costs while maintaining performance.
