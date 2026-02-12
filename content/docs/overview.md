# Overview

> **Last Updated:** February 12, 2026
> **Branch:** `dev13` | **Platform:** iOS 16+ | **Backend:** Google Cloud Run

## What is Crave?

Crave is a recipe management and pantry tracking iOS app that bridges the gap between recipe discovery and kitchen execution.

## Core Capabilities

Users can:

- **Import recipes** from any website or video (YouTube, TikTok, Instagram Reels)
- **Scan ingredients** with the camera using AI-powered detection (Google Gemini Vision)
- **Manage a pantry** with categories (Fridge, Freezer, Pantry)
- **Maintain a grocery list**
- **Explore** public recipes shared by the community

## Business Model

The app follows a **freemium model** with:
- 5 recipe imports per month (free tier)
- 10 ingredient scans per month (free tier)
- Unlimited imports and scans with **Crave Pro** subscription

## Architecture at a Glance

```
┌─────────────────┐
│   iOS App       │
│   (SwiftUI)     │
└────────┬────────┘
         │
         ├─── Firebase (Auth, Firestore, Storage)
         ├─── RevenueCat (Subscriptions)
         ├─── Gemini AI (On-device vision)
         │
         └─── Backend API (FastAPI)
                  │
                  ├─── yt-dlp (Video downloads)
                  ├─── FFmpeg (Audio processing)
                  └─── Gemini AI (Recipe extraction)
```

## Quick Links

- [Tech Stack](/docs/tech-stack) — Technologies and frameworks
- [Architecture](/docs/architecture) — System design and patterns
- [Data Flow](/docs/data-flow) — How data moves through the system
- [Backend API](/docs/backend-api) — API endpoints and contracts
