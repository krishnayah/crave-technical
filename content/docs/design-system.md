# Design System

## Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `background` | `#F2ECDA` | App background (cream/ecru) |
| `primaryText` | `#222222` | Headlines, buttons, primary text |
| `secondaryText` | `#5A5A5A` | Captions, secondary info |
| `accent` | `#B35A3B` | Terracotta highlights |
| `cardBackground` | `#EAE8E4` | Card surfaces |
| `darkCard` | `#333333` | Dark-on-light cards |

### Usage in Code

```swift
Color.craveBackground      // #F2ECDA
Color.cravePrimaryText     // #222222
Color.craveSecondaryText   // #5A5A5A
Color.craveAccent          // #B35A3B
Color.craveCardBackground  // #EAE8E4
Color.craveDarkCard        // #333333
```

## Typography

Custom font system via `.craveFont()` modifier:

### Font Styles

| Style | Size | Weight | Use Case |
|-------|------|--------|----------|
| `.largeTitle` | 34pt | Serif | Large headers, section titles |
| `.sectionHeader` | 22pt | Semibold | Section dividers |
| `.body` | 17pt | Regular | Body text, paragraphs |
| `.caption` | 13pt | Regular | Small text, metadata |
| `.action` | 17pt | Semibold | Button labels, CTAs |

### Usage in Code

```swift
Text("Welcome to Crave")
    .craveFont(.largeTitle)
    .foregroundColor(.cravePrimaryText)

Text("Your recipes")
    .craveFont(.sectionHeader)

Text("Import from any website...")
    .craveFont(.body)
    .foregroundColor(.craveSecondaryText)
```

## Component Library

### Buttons

**Primary Button:**
```swift
Button("Import Recipe") {
    // action
}
.buttonStyle(.bordered)
.tint(.craveAccent)
```

**Secondary Button:**
```swift
Button("Cancel") {
    // action
}
.buttonStyle(.plain)
.foregroundColor(.craveSecondaryText)
```

### Cards

**Recipe Card:**
- Background: `craveCardBackground`
- Corner radius: 16
- Shadow: light, 2pt offset
- Padding: 16pt

**Implementation:**
```swift
VStack {
    // content
}
.padding()
.background(Color.craveCardBackground)
.cornerRadius(16)
.shadow(color: .black.opacity(0.1), radius: 4, y: 2)
```

### Forms

**Text Field:**
```swift
TextField("Recipe URL", text: $url)
    .textFieldStyle(.roundedBorder)
    .padding(.horizontal)
```

**Segmented Picker (Storage Location):**
```swift
Picker("Storage", selection: $location) {
    Text("Fridge").tag(StorageLocation.fridge)
    Text("Freezer").tag(StorageLocation.freezer)
    Text("Pantry").tag(StorageLocation.pantry)
}
.pickerStyle(.segmented)
```

## Spacing System

| Token | Value | Usage |
|-------|-------|-------|
| `xxs` | 4pt | Icon padding |
| `xs` | 8pt | Tight spacing |
| `sm` | 12pt | Default spacing |
| `md` | 16pt | Card padding |
| `lg` | 24pt | Section spacing |
| `xl` | 32pt | Large gaps |

## Iconography

Crave uses **SF Symbols** for all icons:

| Feature | Icon |
|---------|------|
| Home | `house.fill` |
| Recipes | `book.fill` |
| Pantry | `archivebox.fill` |
| Grocery | `cart.fill` |
| Profile | `person.crop.circle.fill` |
| Scan | `camera.fill` |
| Import | `arrow.down.circle.fill` |
| Search | `magnifyingglass` |

## Layout Patterns

### List Pattern

```swift
ScrollView {
    LazyVStack(spacing: 12) {
        ForEach(items) { item in
            ItemRow(item: item)
        }
    }
    .padding()
}
```

### Grid Pattern (Explore Feed)

```swift
ScrollView {
    LazyVGrid(columns: [
        GridItem(.flexible()),
        GridItem(.flexible())
    ], spacing: 16) {
        ForEach(recipes) { recipe in
            RecipeCard(recipe: recipe)
        }
    }
    .padding()
}
```

### Tab Pattern

```swift
TabView {
    HomeView()
        .tabItem {
            Label("Home", systemImage: "house.fill")
        }

    MyRecipesView()
        .tabItem {
            Label("Recipes", systemImage: "book.fill")
        }

    // ... more tabs
}
.tint(.craveAccent)
```

## Animation

### Standard Transitions

```swift
.transition(.opacity.combined(with: .scale))
.animation(.spring(response: 0.3), value: isPresented)
```

### Button Feedback

```swift
.buttonStyle(.borderedProminent)
.animation(.easeInOut(duration: 0.2), value: isPressed)
```

## Dark Mode

Crave currently uses a **light-only color palette**. Colors are defined as static values and do not adapt to system dark mode.

Future consideration: Add dark mode support with:
- Dark background: `#1A1A1A`
- Accent adjustment for better contrast
- Semantic color tokens that adapt automatically
