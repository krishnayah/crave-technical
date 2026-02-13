# Backend API

**Base URL:** `https://crave-backend-512781679428.us-central1.run.app`

## Endpoints

### `POST /extract`

Extract a recipe from a URL (website or video).

#### Request

```json
{
  "url": "https://example.com/recipe"
}
```

#### Response

```json
{
  "title": "Chocolate Chip Cookies",
  "description": "Classic recipe...",
  "image": "https://...",
  "ingredients": [
    { "item": "flour", "quantity": "2 cups" }
  ],
  "instructions": ["Preheat oven...", "Mix dry ingredients..."],
  "prep_time": "15 minutes",
  "cook_time": "12 minutes",
  "servings": "24 cookies"
}
```

#### Flow

1. **URL Detection**: Checks for youtube.com, youtu.be, tiktok.com, instagram.com
2. **Video Path**: yt-dlp downloads audio (with cookies + Deno) → Gemini Files API → Gemini 2.0 Flash extracts recipe
3. **Website Path**: Fetches HTML (truncated to 30K chars) → Gemini 2.0 Flash extracts recipe
4. **Output**: Standardized recipe JSON

### `POST /cancel-subscription`

Cancel a user's RevenueCat subscription via the V2 API.

```json
{ "uid": "firebase-user-id" }
```

Lists subscriptions, finds active one, cancels it. Subscription remains active until end of billing period.

### `GET /health`

Returns `{"status": "ok"}`.

### `GET /`

Returns `{"message": "Crave Backend is Running!"}`.

## Error Codes

| Code | Meaning |
|------|---------|
| `400` | Invalid URL or missing parameters |
| `404` | No subscriptions found |
| `500` | Internal error (Gemini failure, download failure) |

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `GEMINI_API_KEY` | Google Gemini API key | Yes |
| `REVENUECAT_V2_API_KEY` | RevenueCat server-side V2 API key | Yes |
| `REVENUECAT_PROJECT_ID` | RevenueCat project identifier | Yes |
| `YOUTUBE_COOKIES_B64` | Base64-encoded cookies.txt for yt-dlp | No |

## Deployment

The Dockerfile installs Python 3.11-slim, FFmpeg, and Deno (for yt-dlp extractor scripts).

```bash
docker build -t gcr.io/PROJECT_ID/crave-backend ./backend
docker push gcr.io/PROJECT_ID/crave-backend

gcloud run deploy crave-backend \
  --image gcr.io/PROJECT_ID/crave-backend \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

## Local Development

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
# http://localhost:8000
```

## Dependencies

```
fastapi>=0.109.2
uvicorn>=0.27.1
yt-dlp>=2024.04.09
google-generativeai>=0.5.0
python-dotenv>=1.0.1
python-multipart>=0.0.9
ffmpeg-python>=0.2.0
requests>=2.31.0
```
