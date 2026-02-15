---
title: App Features
weight: 2
---

# Core Features

Crave combines on-device machine learning with cloud-based generative AI to deliver a seamless cooking experience.

## Smart Pantry Scanning

The "Receipt Scanning" feature (internally **Smart Pantry**) allows users to quickly populate their digital pantry by scanning grocery items or receipts.

### Technical Implementation

The scanning engine (`VisionService`) utilizes a dual-pipeline approach using Apple's **Vision Framework**. It processes camera frames in parallel to maximize detection accuracy.

#### 1. Hybrid Vision Pipeline

We execute two requests simultaneously on the captured image:

1.  **`VNClassifyImageRequest`**: Identifies visual objects (e.g., "Apple", "Banana") with a confidence threshold > 0.6.
2.  **`VNRecognizeTextRequest`**: Extracts text from packaging (OCR) to identify labeled goods.

```swift
// VisionService.swift
func classifyImage(_ image: UIImage) {
    guard let ciImage = CIImage(image: image) else { return }

    // 1. Image Classification Request
    let classifyRequest = VNClassifyImageRequest()

    // 2. Text Recognition Request (OCR)
    let textRequest = VNRecognizeTextRequest()
    textRequest.recognitionLevel = .accurate

    let handler = VNImageRequestHandler(ciImage: ciImage, orientation: .up)

    DispatchQueue.global(qos: .userInitiated).async {
        try? handler.perform([classifyRequest, textRequest])
        // ... processing logic
    }
}
```

#### 2. Filtering & Normalization

To prevent non-food items from cluttering the pantry, we implement a strict blocklist and normalization layer.

**Blocklist Logic:**

```swift
private let ignoredTerms: Set<String> = [
    "structure", "wood", "processed", "room", "table", "container", "object", "material"
]
```

**Normalization:**
Raw OCR results (e.g., "Goya Golden Corn") are normalized to base ingredients ("Corn") using the `FoodKnowledge.normalize` utility before being added to the `detectedIngredients` array.

## Allergen Detection

Crave maintains a user safety layer by detecting allergens in recipes using specific keyword matching.

### Supported Allergens

The app supports detection for **16 common allergens**:

1.  **Dairy** (Milk, cheese, whey, casein)
2.  **Eggs** (Egg white, yolk, mayo)
3.  **Peanuts**
4.  **Tree Nuts** (Almond, cashew, walnut)
5.  **Wheat**
6.  **Gluten** (Barley, rye, malt)
7.  **Soy** (Tofu, edamame, miso)
8.  **Fish**
9.  **Shellfish** (Shrimp, crab, lobster)
10. **Sesame**
11. **Mustard**
12. **Celery**
13. **Lupin**
14. **Mollusks**
15. **Sulfites**
16. **Corn**

### Detection Implementation

The `AllergenManager` scans ingredient strings against a predefined database of keywords associated with each allergen.

```swift
// AllergenManager.swift

/// Check a single ingredient name against user's allergens
func detectAllergens(in ingredientName: String) -> [Allergen] {
    let lower = ingredientName.lowercased()
    return selectedAllergens.filter { allergen in
        allergen.keywords.contains { keyword in
            lower.contains(keyword)
        }
    }
}
```

## "Spice It Up"

Powered by **Gemini 2.0 Flash**, "Spice It Up" is an AI creative engine that suggests recipe modifications based on the user's available pantry items.

### The Prompt

The core of this feature is a carefully engineered prompt that instructs Gemini to act as a "creative chef assistant".

```text
You are a creative chef assistant. Given a recipe and a list of pantry items the user has, suggest 4 creative modification plans to "spice up" the recipe.

RECIPE:
Title: \(recipe.title)
Ingredients: \(ingredientNames.joined(separator: ", "))
Instructions: \(instructionsList.joined(separator: ". "))

USER'S PANTRY ITEMS: \(pantryNames.joined(separator: ", "))

RULES:
- Return exactly 4 modification plans
- Each plan should have a distinct theme (e.g. spicier, healthier, more protein, fusion twist, comfort food, etc.)
- Plans CAN use items from the pantry but DO NOT have to — feel free to suggest ingredients the user doesn't have
- For each added ingredient, set "inPantry" to true ONLY if that ingredient name closely matches something in the user's pantry
- Return ONLY raw JSON. No markdown, no commentary.
```

### JSON Structure

The AI response is parsed into `ModificationPlan` objects using Swift's `Codable` interface.

```swift
struct ModificationPlan: Identifiable {
    let title: String
    let emoji: String
    let description: String
    let addedIngredients: [PlanIngredient]
    let additionalInstructions: [String]

    var hasAnyPantryItems: Bool {
        addedIngredients.contains(where: { $0.inPantry })
    }
}
```

## Enhanced Onboarding

The onboarding flow (`OnboardingView`) guides users through the app's value proposition and uses `AppStorage` to track completion status.

1.  **Welcome**: "Your personal recipe manager."
2.  **Effortless Imports**: Import from Instagram/TikTok.
3.  **Smart Pantry**: Introduction to camera scanning.
4.  **Premium Paywall**: Conversion screen for Crave Pro.

The flow uses a `TabView` with page indicators and custom "Next/Skip" navigation logic.
