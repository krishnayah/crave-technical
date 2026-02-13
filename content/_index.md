---
title: Crave
layout: hextra-home
---

<div class="hx-mt-6 hx-mb-2">
{{< hextra/hero-headline >}}
  Crave: From Idea to Recipe.
{{< /hextra/hero-headline >}}
</div>

<h2 style="margin-top: 0.25rem; font-weight: 200;">An AI-powered culinary companion for iOS. From idea to plate.</h2>

<div class="hx-mt-10 hx-mb-6">
<a href="docs" class="crave-btn-primary">Read our docs.</a>
</div>

---

## What Crave Does

Crave is a native iOS app that turns any recipe source into a structured, searchable personal cookbook. Users can import recipes from websites, YouTube, TikTok, and Instagram -- or generate original recipes with AI. They can point their camera at ingredients for instant identification, manage pantry inventory, build grocery lists, and share recipes on a community feed.

The goal is simple: get out of the way and let people cook.

---

## Core Feature Stack

- **Universal Recipe Import** -- Import from any URL. Websites are parsed via Gemini for structured recipe data. Video content from YouTube, TikTok, and Instagram is processed through our backend -- audio is extracted via yt-dlp and analyzed by Gemini. A Share Extension allows importing directly from Safari or any app's share sheet.

- **AI Recipe Generation** -- Pick a cuisine category (14 options from Chinese to High Protein to Surprise Me) and Gemini generates a complete recipe with ingredients, steps, and timing. Recipes are auto-tagged and saved directly to the user's collection.

- **Spice It Up** -- Select any saved recipe and get 4 AI-generated modification plans. Each plan suggests new ingredients and additional steps, cross-referenced against your pantry to highlight what you already have.

- **Smart Ingredient Scanning** -- Point the camera at a shelf, countertop, or open fridge. Gemini 2.5 Pro Vision identifies ingredients with category classification. Results feed directly into pantry tracking.

- **Pantry & Grocery Management** -- Track what you have across Fridge, Freezer, and Pantry. Build grocery lists with live badge counts. All data syncs in real-time via Firestore.

- **Explore Feed** -- A community recipe feed with tag-based filtering. Browse by cuisine, diet, meal type, or characteristics. Save others' recipes or share your own.

- **AI Kitchen Assistant** -- Ask Chef Gemini any cooking question. Techniques, substitutions, meal planning, food science.

---

## Technical Architecture

| Layer            | Technology           | Purpose                                          |
| ---------------- | -------------------- | ------------------------------------------------ |
| **Frontend**     | SwiftUI              | Native iOS UI with declarative layout            |
| **Auth**         | Firebase Auth        | Email/password + Google Sign-In                  |
| **Database**     | Cloud Firestore      | Real-time document store with offline support    |
| **Storage**      | Firebase Storage     | Recipe images and user media                     |
| **AI (Vision)**  | Gemini 2.5 Pro       | On-device ingredient scanning from photos        |
| **AI (Text)**    | Gemini 2.0 Flash     | Recipe generation, tagging, search, spice-it-up  |
| **Payments**     | RevenueCat           | Subscription management and entitlements         |
| **Backend**      | FastAPI on Cloud Run | Recipe extraction from URLs (video + web)        |

The app follows MVVM architecture with singleton services. ViewModels never talk to Firebase directly -- they go through dedicated service classes.

---

## Design Philosophy

- **Native-first.** No cross-platform abstractions. SwiftUI gives full access to iOS conventions, animations, and performance.

- **Privacy-first.** User data lives in their own Firestore document tree. No analytics harvesting, no ad tracking. Gemini API calls are stateless.

- **Offline-capable.** Firestore's offline persistence means the app works without connectivity. A NetworkMonitor banner alerts users when offline.

- **Minimal UI.** Cream/ecru palette, serif headings, clean tab bar. Light-mode only. Haptic feedback on key interactions.

---

## Monetization

Crave uses a freemium model managed through RevenueCat:

|                              | Free        | Crave Pro   |
| ---------------------------- | ----------- | ----------- |
| Recipe imports               | 5/month     | Unlimited   |
| AI ingredient scans          | 10/month    | Unlimited   |
| AI recipe generation         | Unlimited   | Unlimited   |
| Manual recipe entry          | Unlimited   | Unlimited   |
| Pantry and grocery features  | Full access | Full access |

RevenueCat handles all subscription lifecycle events: purchases, renewals, cancellations, and entitlement checks.

---

## Project Philosophy

- **Keep it simple.** Every feature ships complete or not at all. No half-baked implementations behind feature flags.

- **Complexity budget.** Fewer, well-chosen tools over a sprawling dependency graph. The entire app runs on four core services: Firebase, Gemini, RevenueCat, and native SwiftUI.

- **Documentation throughout.** This site exists because the architecture should be legible to anyone reading the code for the first time.

<div class="hx-mt-12">
<a href="docs/overview" class="crave-btn-secondary">Read the Documentation</a>
</div>
