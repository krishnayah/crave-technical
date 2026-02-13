# Deployment

## iOS App

### Requirements

- Xcode (latest stable version)
- iOS 16+ deployment target
- Apple Developer account
- Swift Package Manager (for dependencies)

### Build Process

1. Open `Crave-iOS/Crave/Crave/Crave.xcodeproj`
2. Configure signing (select development team)
3. Dependencies resolve automatically via SPM
4. Product → Archive → Distribute App

### Share Extension

The CraveShare extension requires:
- Shared `GoogleService-Info.plist` in extension target
- Keychain Sharing capability (for Firebase Auth)

## Backend (Cloud Run)

### Docker Image

Installs Python 3.11-slim, FFmpeg, and Deno (for yt-dlp extractor scripts).

```bash
docker build -t gcr.io/PROJECT_ID/crave-backend ./backend
docker push gcr.io/PROJECT_ID/crave-backend
```

### Deploy

```bash
gcloud run deploy crave-backend \
  --image gcr.io/PROJECT_ID/crave-backend \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars GEMINI_API_KEY=...,REVENUECAT_V2_API_KEY=...,REVENUECAT_PROJECT_ID=...
```

### Scaling

```bash
gcloud run services update crave-backend \
  --min-instances 0 \
  --max-instances 100 \
  --concurrency 80
```

### Rollback

```bash
gcloud run revisions list --service crave-backend
gcloud run services update-traffic crave-backend \
  --to-revisions REVISION_NAME=100
```

### Health Check

```bash
curl https://crave-backend-512781679428.us-central1.run.app/health
# {"status": "ok"}
```
