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
    {
      "item": "flour",
      "quantity": "2 cups"
    }
  ],
  "instructions": [
    "Preheat oven...",
    "Mix dry ingredients..."
  ],
  "prep_time": "15 minutes",
  "cook_time": "12 minutes",
  "servings": "24 cookies"
}
```

#### Flow

1. **URL Detection**: Determines if URL is video (YouTube/TikTok/Instagram) or website
2. **Video Path**:
   - Downloads audio via `yt-dlp`
   - Sends audio to Gemini for transcription + extraction
3. **Website Path**:
   - Fetches HTML content
   - Sends HTML to Gemini for structured extraction
4. **Structured Output**: Returns standardized recipe JSON

#### Supported Platforms

**Video:**
- YouTube
- TikTok
- Instagram Reels

**Websites:**
- Any recipe website with structured data
- Food blogs
- Personal recipe pages

### `POST /cancel-subscription`

Cancel a user's RevenueCat subscription via the V2 API.

#### Request

```json
{
  "uid": "firebase-user-id"
}
```

#### Response

```json
{
  "success": true,
  "message": "Subscription cancelled"
}
```

### `GET /health`

Health check endpoint.

#### Response

```json
{
  "status": "healthy",
  "timestamp": "2026-02-12T10:30:00Z"
}
```

## Error Handling

### Standard Error Response

```json
{
  "detail": "Error message describing what went wrong"
}
```

### Common Error Codes

| Code | Meaning |
|------|---------|
| `400` | Bad request (invalid URL, missing parameters) |
| `404` | Resource not found |
| `429` | Rate limit exceeded |
| `500` | Internal server error |
| `503` | Service unavailable (Gemini API down) |

## Rate Limiting

The backend does not currently implement rate limiting. Usage is gated on the client side via RevenueCat subscription checks.

**Future consideration:**
- Add per-user rate limiting
- Implement Redis-based quota tracking
- Return `429` when limits exceeded

## Authentication

Currently, the API is **unauthenticated**. All requests are accepted.

**Security considerations:**
- Add Firebase Auth token verification
- Validate user ID in request body matches token
- Rate limit by user ID

## Environment Variables

| Variable | Description |
|----------|-------------|
| `GEMINI_API_KEY` | Google Gemini API key |
| `REVENUECAT_V2_API_KEY` | RevenueCat server-side V2 API key |
| `REVENUECAT_PROJECT_ID` | RevenueCat project identifier |

## Deployment

### Docker Build

```bash
docker build -t gcr.io/PROJECT_ID/crave-backend ./backend
docker push gcr.io/PROJECT_ID/crave-backend
```

### Cloud Run Deploy

```bash
gcloud run deploy crave-backend \
  --image gcr.io/PROJECT_ID/crave-backend \
  --platform managed \
  --region us-central1 \
  --set-env-vars GEMINI_API_KEY=...,REVENUECAT_V2_API_KEY=...,REVENUECAT_PROJECT_ID=...
```

### Environment Setup

Create `.env` file in `backend/` directory:

```env
GEMINI_API_KEY=your_key_here
REVENUECAT_V2_API_KEY=your_key_here
REVENUECAT_PROJECT_ID=your_project_id
```

## Local Development

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```

Server runs on `http://localhost:8000`

### Testing the API

```bash
# Health check
curl http://localhost:8000/health

# Extract recipe
curl -X POST http://localhost:8000/extract \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/recipe"}'
```

## Dependencies

From `requirements.txt`:

```
fastapi
uvicorn[standard]
google-generativeai
yt-dlp
ffmpeg-python
requests
python-dotenv
```

## API Design Principles

1. **Simple REST**: Standard HTTP verbs and JSON payloads
2. **Stateless**: No session management, each request is independent
3. **Fast Fail**: Return errors quickly rather than hanging
4. **Structured Output**: Consistent JSON schema across all responses
5. **Idempotent**: Repeated calls with same input produce same output
