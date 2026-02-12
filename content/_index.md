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

Crave is a native iOS app that turns any recipe source into a structured, searchable personal cookbook. Users can import recipes from websites, YouTube, TikTok, and Instagram. They can point their camera at ingredients and have them identified instantly. Crave manages pantry inventory, generates grocery lists, and connects users to a community recipe feed.

The goal is simple: get out of the way and let people cook. Every feature is designed around reducing friction between finding a recipe and having dinner on the table.

---

## Core Feature Stack

- **Universal Recipe Import** -- Import from any source. Websites are parsed for structured recipe data. Video content from YouTube, TikTok, and Instagram Reels is processed through our AI backend, which extracts ingredients, steps, and timing from both audio and visual content. The result is a clean, standardized recipe card regardless of the original format.

- **Smart Ingredient Scanning** -- Point the camera at a shelf, countertop, or open fridge. Google Gemini Vision identifies ingredients in real time with high accuracy. No manual entry required. Results feed directly into pantry tracking and recipe suggestions.

- **Pantry Management** -- Track what you have across three storage zones: Fridge, Freezer, and Pantry. Items are automatically categorized and expiration-aware. The system cross-references your pantry with saved recipes to surface what you can cook right now.

- **Grocery Lists** -- Build shopping lists directly from recipes. The app diffs your pantry against recipe requirements so you only buy what you actually need. Lists can be shared with household members in real time via Firebase sync.

- **Explore Feed** -- A curated, community-driven recipe feed. Browse what others are cooking, save recipes to your collection, and discover new cuisines. The feed is algorithmically sorted but not ad-driven.

---

## Technical Architecture

Crave is built on a modern, mobile-first stack with clear separation of concerns:

| Layer        | Technology              | Purpose                                               |
| ------------ | ----------------------- | ----------------------------------------------------- |
| **Frontend** | SwiftUI                 | Native iOS UI with declarative layout                 |
| **Auth**     | Firebase Auth           | Email, Apple Sign-In, Google Sign-In                  |
| **Database** | Cloud Firestore         | Real-time document store with offline support         |
| **Storage**  | Firebase Storage        | Recipe images and user media                          |
| **AI**       | Google Gemini 2.5 Flash | Recipe extraction, ingredient vision, content parsing |
| **Payments** | RevenueCat              | Subscription management and entitlements              |
| **Backend**  | Cloud Functions         | Server-side recipe processing and API orchestration   |

The app follows MVVM architecture with a service-oriented data layer. ViewModels never talk to Firebase directly -- they go through dedicated service classes that handle caching, error recovery, and offline fallback.

---

## Design Philosophy

- **Native-first.** No cross-platform abstractions. SwiftUI gives us full access to iOS conventions, animations, and performance characteristics. The app feels like it belongs on the platform.

- **Privacy-first.** User data lives in their own Firestore document tree. No analytics harvesting, no ad tracking, no third-party data sharing. Gemini API calls are stateless and don't retain user content.

- **Offline-capable.** Firestore's offline persistence means the app works without connectivity. Recipes, pantry data, and grocery lists are available offline with automatic sync when connection returns.

- **Minimal UI.** The interface is deliberately sparse. No onboarding carousels, no tooltip tours, no gamification. Users open the app, import a recipe, and start cooking.

---

## Monetization

Crave uses a freemium model managed through RevenueCat:

|                              | Free        | Crave Pro   |
| ---------------------------- | ----------- | ----------- |
| Recipe imports               | 5/month     | Unlimited   |
| AI ingredient scans          | 10/month    | Unlimited   |
| Manual recipe entry          | Unlimited   | Unlimited   |
| Pantry and grocery features  | Full access | Full access |
| Priority support             | --          | Included    |
| Early access to new features | --          | Included    |

RevenueCat handles all subscription lifecycle events: purchases, renewals, cancellations, grace periods, and cross-platform entitlement checks. The integration is webhook-driven with server-side receipt validation.

---

## Project Philosophy

This is an opinionated project. A few guiding principles:

- **Keep it simple, stupid.** Every feature ships complete or not at all. There's no half-baked implementations behind feature flags. And if a feature isn't fully ready, it doesn't exist in the build.

- **Complexity budget.** Every dependency, abstraction, and configuration option has a cost. We prefer fewer, well-chosen tools over a sprawling dependency graph. The entire app runs on four core services: Firebase, Gemini, RevenueCat, and native SwiftUI.

- **Documentation throughout.** This site exists because the architecture should be legible to anyone reading the code for the first time. If something isn't documented here, it should be self-evident from the source. We spent lots of time documenting our process to ensure that we could expand our product to a team of developers and maintain a high level of quality.

<div class="hx-mt-12">
<a href="docs/overview" class="crave-btn-secondary">Read the Documentation</a>
</div>
