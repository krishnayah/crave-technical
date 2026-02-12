# RevenueCat Implementation

## Configuration

**Initialized in `CraveApp.swift`:**

```swift
Purchases.logLevel = .debug
Purchases.configure(withAPIKey: "test_lJYnWXGbILlbvECTRQalKpVnQTx")
```

**Entitlement name:** `"Crave Pro"`

## SubscriptionManager

Located at `Managers/SubscriptionManager.swift`. This is the **central hub** for all subscription logic.

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `isPremium` | `Bool` | Whether user has active "Crave Pro" entitlement |
| `currentOffering` | `Offering?` | RevenueCat's current offering for the paywall |
| `currentImports` | `Int` | Monthly import count (persisted via `@AppStorage`) |
| `currentScans` | `Int` | Monthly scan count (persisted via `@AppStorage`) |
| `canImport` | `Bool` | `isPremium \|\| currentImports < 5` |
| `canScan` | `Bool` | `isPremium \|\| currentScans < 10` |

### Free Tier Limits

| Feature | Free Limit | Premium |
|---------|-----------|---------|
| Recipe Imports | 5/month | Unlimited |
| Ingredient Scans | 10/month | Unlimited |
| Manual Entry | Unlimited | Unlimited |

### Monthly Reset Logic

- `lastResetDate` stored via `@AppStorage`
- On each app launch, `checkResetCycle()` compares current month to stored date
- If different month → reset both counters to 0

**Implementation:**

```swift
func checkResetCycle() {
    let calendar = Calendar.current
    let now = Date()

    if let lastReset = lastResetDate {
        if !calendar.isDate(now, equalTo: lastReset, toGranularity: .month) {
            // New month detected
            currentImports = 0
            currentScans = 0
            lastResetDate = now
            print("📅 [USAGE] Monthly cycle reset")
        }
    } else {
        lastResetDate = now
    }
}
```

## Paywall Presentation

The paywall is triggered in two places:

### 1. RecipeImportView

When `importRecipe()` is called and `canImport == false`:
- Sets `showPaywall = true`
- Presents `SubscriptionPaywallView` as a sheet

### 2. PantryView

When "Scan Item" is tapped and `canScan == false`:
- Sets `showingLimitAlert = true`
- Presents `SubscriptionPaywallView` as a sheet

## PaywallView

Located at `Views/Paywall/PaywallView.swift`.

A wrapper around RevenueCat's built-in `PaywallView`:
- Passes the current `Offering` from `SubscriptionManager`
- Handles `onPurchaseCompleted` and `onRestoreCompleted` callbacks
- Shows loading, error, and empty states

**Key Features:**
- Automatically displays pricing from RevenueCat dashboard
- Handles purchase flow end-to-end
- Supports subscription restore
- Updates `isPremium` state on successful purchase

## Customer Center

In `ProfileView`, the "Manage Subscription" button presents RevenueCat's `CustomerCenterView`:

```swift
.sheet(isPresented: $showCustomerCenter) {
    CustomerCenterView()
}
```

This provides users with:
- Current subscription status
- Billing information
- Cancellation options
- Restore purchases

## Onboarding Paywall

The last slide of the 4-slide onboarding flow (`OnboardingPaywallSlide`) includes:

- The RevenueCat `PaywallView` (no close button)
- Free plan info: "5 recipe imports & 10 ingredient scans per month"
- "Continue with Free Plan" button to skip
- Restore Purchases, Terms, and Privacy links
- Page dots are hidden on this slide

This soft paywall approach:
- Educates users about premium features
- Doesn't force subscription
- Maintains a positive first impression

## Debug Logging

All usage tracking includes console logging (prefix `[USAGE]`, `[PAYWALL]`, `[IMPORT]`, `[SCAN]`):

```
📊 [USAGE] Current counts — imports: 3/5, scans: 7/10
📦 [USAGE] canImport: true | imports: 3/5 | premium: false
✅ [IMPORT] Recipe imported successfully, incrementing count
📦 [USAGE] Import incremented → 4/5
🚫 [USAGE] IMPORT LIMIT REACHED — next import will show paywall
```

This makes debugging subscription logic straightforward during development.

## Best Practices

1. **Always check before incrementing**: Verify `canImport` or `canScan` before starting the operation
2. **Increment immediately after success**: Update counters as soon as the operation completes
3. **Show paywall proactively**: Don't wait for errors — gate the feature upfront
4. **Respect the reset cycle**: Check monthly reset on every app launch
5. **Log everything**: Use console logging to track subscription state changes
