# Overview

> **Last Updated:** February 12, 2026
> **Platform:** iOS 16+ | **Backend:** Google Cloud Run

## What is Crave?

Crave is a recipe management and pantry tracking iOS app that bridges the gap between recipe discovery and kitchen execution.

## Core Capabilities

Users can:

- **Import recipes** from any website or video (YouTube, TikTok, Instagram) -- including via Share Extension
- **Generate recipes** from 14 cuisine categories using AI (Gemini 2.0 Flash)
- **Modify recipes** with AI-powered "Spice It Up" suggestions based on pantry contents
- **Scan ingredients** with the camera using Gemini 2.5 Pro Vision
- **Manage a pantry** with categories (Fridge, Freezer, Pantry)
- **Maintain a grocery list** with real-time badge counts
- **Explore** public recipes shared by the community, filtered by tags
- **Ask Chef Gemini** cooking questions via the AI Kitchen Assistant
- **Track allergens** with a 16-allergen detection system

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
         ├─── Gemini 2.5 Pro (Vision / ingredient scanning)
         ├─── Gemini 2.0 Flash (Text / generation, tagging, search)
         │
         └─── Backend API (FastAPI on Cloud Run)
                  │
                  ├─── yt-dlp + Deno (Video downloads)
                  ├─── FFmpeg (Audio extraction)
                  └─── Gemini 2.0 Flash (Recipe extraction)
```

## Quick Links

- [Tech Stack](/docs/tech-stack) — Technologies and frameworks
- [Architecture](/docs/architecture) — System design and patterns
- [Data Flow](/docs/data-flow) — How data moves through the system
- [Backend API](/docs/backend-api) — API endpoints and contracts
