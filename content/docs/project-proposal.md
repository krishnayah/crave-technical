# CRAVE — FROM CRAVING TO COOKING

_Written Proposal for the RevenueCat Shipyard 2026 Hackathon_

---

## The Problem

Food inspiration is everywhere. From YouTube videos, TikToks, Instagram reels, NYT Cooking articles, people are constantly saving recipes they swear they'll make "next week." They almost never do.

We know this firsthand. We (David, Jordan, and Kyle) are college students at Rensselaer Polytechnic Institute living off-campus with no meal plan. We send each other cooking videos *constantly*, but we forget them just as fast. Vegetables rot in the back of the fridge. Duplicate groceries pile up. Dinner ends up being rice and whatever protein we can find.

## Built for a broader audience
This isn't just a college student problem. Families waste food because no one knows what's already in the pantry. 

Our moms all personally struggle with the same five-meal rotation because turning a recipe idea into an actual grocery run feels like too much friction. After coming home from work, they just want to relax and not stress about finding good recipes. (In fact, some of our moms send us TikTok cooking reels!)

We've optimized our app for college students and younger parents (18-34) who oftentimes get home from a 9-5 schedule, and start scrolling on TikTok to relax. They oftentimes see recipes that they would like to make, and this app turns every potential scroll into an actionable recipe.

And for people managing allergies (or dietary restrictions), every new recipe requires manually scanning through a dozen ingredient lists for a single allergen. My friends often have trouble finding kosher or vegetarian options.

The gap between "I want to cook this" and "I'm actually cooking this" is a planning problem. Crave is designed to close it.

---

## What Crave Does

Crave is a native iOS app that connects the entire journey from recipe discovery to dinner on the table across five systems:

**Universal Recipe Import**: Share any website link or TikTok. Our app extracts a clean, structured recipe card. For video content, our AI backend processes both audio and visuals to identify ingredients, steps, and timing with no manual entry.

**Smart Pantry Management**: Point your camera at your fridge or pantry shelf and Crave's computer vision (Google Gemini Vision) identifies every ingredient in the frame, organized across Fridge, Freezer, and Pantry zones. The system then surfaces what you can cook _right now_ with zero extra shopping.

**Spice It Up**: Sometimes I want to make a dish spicier, or add a bit more tang. That's why we created a 'spice it up' feature. Select any saved recipe, tap "Spice It Up," and Crave's AI suggests modifications based off what you own in your pantry: add heat with chili flakes you already have, boost protein with leftover chicken, deepen umami with the soy sauce in your cabinet door. We wanted to make it easy to create variety in meals, with no effort.

**AI Recipe Generation**: Tell us what cuisine you're CRAVE-ing (get it?) and it generates a full recipe optimized around your pantry. Want something high-protein on a weeknight? Specify it. The result isn't pulled from a static database, but rather it reasons over your actual inventory and produces something you can genuinely cook tonight, with a grocery list for the gaps.

**Allergen Awareness**: Every recipe Crave imports, generates, or modifies is automatically scanned against major allergen groups. Users set their profile once during onboarding; from that point on, Crave treats allergen constraints as hard requirements. A peanut allergy user will never see a Spice It Up recommendation that involves peanut oil.

We've also integrated a community feature through an explore feed, where you can see user-submitted recipes and save them for your own cooking.

---

## Monetization

Crave runs a freemium model managed through RevenueCat.

**Free:** 5 recipe imports/month, 10 AI pantry scans/month, unlimited manual entry, full allergen screening, full grocery list and Explore access.

**Crave Pro ($9.99/month) or (79.99/year):** Unlimited imports, scans, Spice It Up suggestions, and AI recipe generation. Priority support and early access to new features.

The free tier is genuinely useful for a casual home cook. Limits are placed specifically on the AI-heavy features (video processing, camera scanning, generative recipes) where real cost exists. Weekly meal planners and households cooking new recipes regularly will hit those limits and find Pro worth it.

There is also a **14 day free trial** for users to experience 14 days of the premium features!

---

## Tech Stack

| Layer       | Technology                           |
| ----------- | ------------------------------------ |
| Frontend    | SwiftUI (native iOS)                 |
| Auth        | Firebase Auth (Email, Apple, Google) |
| Database    | Cloud Firestore. Firestore for image storage                      |
| AI / Vision | Google Gemini 2.5 Flash/Pro              |
| Payments    | RevenueCat                           |
| Backend     | Firebase Cloud Functions, Docker, Python, OpenAI Whisper, Google Cloud Project             |

---

## What sets Crave apart

Most recipe apps are just databases with a search bar. Crave is a cooking assistant that meets users where they are: on their phone, watching food content, standing in front of an open fridge, trying to figure out what's for dinner.

The Spice It Up engine, allergen-aware generation, and pantry-first recipe logic are features weaved throughout our app. The whole system is designed around one outcome: you open Crave with an idea, and thirty minutes later you're eating something you actually wanted to make.

Crave delivers it.
