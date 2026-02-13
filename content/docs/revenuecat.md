# RevenueCat Implementation

## Configuration

Initialized in `CraveApp.swift`:

```swift
Purchases.logLevel = .debug
Purchases.configure(withAPIKey: "appl_...")
```

**Entitlement name:** `"Crave Pro"` (falls back to any active entitlement)

## SubscriptionManager

Located at `Managers/SubscriptionManager.swift`. Central hub for all subscription logic.

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `isPremium` | `Bool` | Whether user has any active entitlement |
| `subscriptionExpirationDate` | `Date?` | Current subscription expiration |
| `willRenew` | `Bool` | Whether subscription auto-renews |
| `managementURL` | `URL?` | Apple subscription management URL |
| `currentOffering` | `Offering?` | RevenueCat's current offering |
| `currentImports` | `Int` | Monthly import count (`@AppStorage`) |
| `currentScans` | `Int` | Monthly scan count (`@AppStorage`) |
| `canImport` | `Bool` | `isPremium \|\| currentImports < 5` |
| `canScan` | `Bool` | `isPremium \|\| currentScans < 10` |

### Free Tier Limits

| Feature | Free Limit | Premium |
|---------|-----------|---------|
| Recipe Imports | 5/month | Unlimited |
| Ingredient Scans | 10/month | Unlimited |
| Manual Entry | Unlimited | Unlimited |
| AI Generation | Unlimited | Unlimited |

### Monthly Reset Logic

`lastResetDate` stored via `@AppStorage` as `TimeInterval`. On init, `checkResetCycle()` compares current month to stored date. Different month resets both counters to 0.

### Subscription Cancellation

Handled server-side via `POST /cancel-subscription`. iOS sends Firebase UID to backend, which cancels via RevenueCat V2 API. Subscription remains active until end of billing period.

## Paywall

Triggered when `canImport == false` (RecipeImportView) or `canScan == false` (PantryView). Uses RevenueCat's built-in `PaywallView` wrapped in a custom sheet.

## Debug Logging

```
📊 [USAGE] Current counts — imports: 3/5, scans: 7/10
📦 [USAGE] canImport: true | imports: 3/5 | premium: false
📦 [USAGE] Import incremented → 4/5
🚫 [USAGE] IMPORT LIMIT REACHED — next import will show paywall
```
