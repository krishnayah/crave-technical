# Deployment

## iOS App

### Requirements

- Xcode (latest stable version)
- iOS 16+ deployment target
- Apple Developer account
- Swift Package Manager (for dependencies)

### Build Process

1. **Open Project**
   ```bash
   cd Crave-iOS/Crave/Crave
   open Crave.xcodeproj
   ```

2. **Configure Signing**
   - Select project in Navigator
   - Choose target → Signing & Capabilities
   - Select your development team
   - Xcode will automatically manage provisioning profiles

3. **Install Dependencies**
   - Dependencies are managed via Swift Package Manager
   - Xcode will automatically resolve packages on build
   - Packages are defined in `Package.swift`

4. **Archive for Distribution**
   - Product → Archive
   - Wait for archive to complete
   - Organizer window will open automatically

5. **Upload to App Store Connect**
   - Click "Distribute App"
   - Choose "App Store Connect"
   - Select distribution options
   - Upload and wait for processing

### TestFlight

1. In App Store Connect, navigate to your app
2. Select TestFlight tab
3. Add internal testers (up to 100)
4. Add external testers (up to 10,000, requires App Review)
5. Testers receive email invitation
6. They download TestFlight app and install build

### Production Release

1. In App Store Connect, create new version
2. Fill out App Information:
   - Screenshots (6.7", 6.5", 5.5")
   - Description
   - Keywords
   - Support URL
   - Privacy Policy
3. Select build from TestFlight
4. Submit for App Review
5. Wait for approval (typically 24-48 hours)
6. Release manually or automatically on approval

## Backend (Cloud Run)

### Requirements

- Google Cloud account
- `gcloud` CLI installed
- Docker installed
- Project with billing enabled

### Initial Setup

```bash
# Authenticate
gcloud auth login

# Set project
gcloud config set project YOUR_PROJECT_ID

# Enable required APIs
gcloud services enable run.googleapis.com
gcloud services enable containerregistry.googleapis.com
```

### Build and Deploy

```bash
# Navigate to backend directory
cd backend

# Build Docker image
docker build -t gcr.io/YOUR_PROJECT_ID/crave-backend .

# Push to Google Container Registry
docker push gcr.io/YOUR_PROJECT_ID/crave-backend

# Deploy to Cloud Run
gcloud run deploy crave-backend \
  --image gcr.io/YOUR_PROJECT_ID/crave-backend \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars GEMINI_API_KEY=...,REVENUECAT_V2_API_KEY=...,REVENUECAT_PROJECT_ID=...
```

### Environment Variables

Set via Cloud Run console or `gcloud`:

```bash
gcloud run services update crave-backend \
  --set-env-vars GEMINI_API_KEY=your_key,REVENUECAT_V2_API_KEY=your_key,REVENUECAT_PROJECT_ID=your_id
```

### Custom Domain

```bash
# Map custom domain
gcloud run domain-mappings create \
  --service crave-backend \
  --domain api.crave.app \
  --region us-central1
```

### Monitoring

- **Cloud Run Console**: View logs, metrics, and request counts
- **Cloud Logging**: Advanced log filtering and analysis
- **Cloud Monitoring**: Set up alerts for errors and latency

### Scaling Configuration

```bash
gcloud run services update crave-backend \
  --min-instances 0 \
  --max-instances 100 \
  --concurrency 80
```

**Recommendations:**
- `min-instances: 0` — Scale to zero when idle
- `max-instances: 100` — Cap spending
- `concurrency: 80` — Requests per container

### Cost Optimization

Cloud Run pricing:
- **CPU**: Billed only while processing requests
- **Memory**: Billed while container is running
- **Requests**: $0.40 per million requests

**Cost-saving tips:**
- Keep `min-instances: 0` for low-traffic apps
- Optimize container startup time
- Cache Gemini responses where possible
- Monitor and set budget alerts

## CI/CD (Future)

### GitHub Actions for Backend

```yaml
name: Deploy to Cloud Run

on:
  push:
    branches:
      - main
    paths:
      - 'backend/**'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v0
        with:
          service_account_key: ${{ secrets.GCP_SA_KEY }}
          project_id: ${{ secrets.GCP_PROJECT_ID }}

      - name: Build and push Docker image
        run: |
          docker build -t gcr.io/${{ secrets.GCP_PROJECT_ID }}/crave-backend ./backend
          docker push gcr.io/${{ secrets.GCP_PROJECT_ID }}/crave-backend

      - name: Deploy to Cloud Run
        run: |
          gcloud run deploy crave-backend \
            --image gcr.io/${{ secrets.GCP_PROJECT_ID }}/crave-backend \
            --region us-central1 \
            --platform managed
```

### Fastlane for iOS

```ruby
# Fastfile
lane :beta do
  build_app(scheme: "Crave")
  upload_to_testflight
  slack(message: "New build uploaded to TestFlight!")
end

lane :release do
  build_app(scheme: "Crave")
  upload_to_app_store
  slack(message: "App submitted for review!")
end
```

## Configuration Management

### iOS Secrets

**Never commit `Secrets.swift` to Git!**

Add to `.gitignore`:
```
Crave/Secrets.swift
GoogleService-Info.plist
```

Use environment-specific configs:
```swift
#if DEBUG
static let backendUrl = "http://localhost:8000"
#else
static let backendUrl = "https://crave-backend-....run.app"
#endif
```

### Backend Secrets

Use `.env` for local development:
```env
GEMINI_API_KEY=...
REVENUECAT_V2_API_KEY=...
REVENUECAT_PROJECT_ID=...
```

**Never commit `.env` to Git!**

For production, use Cloud Run environment variables.

## Rollback Procedures

### iOS

- In App Store Connect, you cannot "rollback" a live version
- Instead, submit a new build with the previous code
- TestFlight allows switching between builds instantly

### Backend

```bash
# List revisions
gcloud run revisions list --service crave-backend

# Route traffic to previous revision
gcloud run services update-traffic crave-backend \
  --to-revisions REVISION_NAME=100
```

## Health Checks

### Backend

```bash
curl https://crave-backend-....run.app/health
```

Expected response:
```json
{"status": "healthy", "timestamp": "2026-02-12T10:30:00Z"}
```

### iOS

- Monitor crash reports in App Store Connect
- Use Firebase Crashlytics for detailed stack traces
- Check RevenueCat dashboard for subscription issues
