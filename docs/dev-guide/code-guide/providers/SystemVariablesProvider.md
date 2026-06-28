# SystemVariablesProvider

> A `ChangeNotifier` provider shared across the entire app. Used to hold global variables accessible from distant parts of the codebase.

---

## 📦 Properties

| Symbol | Property | Type | Default | Description |
|---|---|---|---|---|
| 🔒 | `_backgroundWidgetVisibility` | `bool` | `false` | Controls whether the `BackgroundWidget` is visible and its `GestureDetector`s are functional |
| 🔒 | `_appCurrentState` | `SystemState` | `SystemState.gatheringFromLoc` | Drives what the Foreground and Background widgets display |
| 🔒 | `_backgroundWidgetColor` | `Color` | `Colors.white` | The current color of the background widget |
| 🟢 | `currentLoc` | `LatLng` | `LatLng(0, 0)` | The user's current map location |

---

## 🔍 Getters

| Symbol | Getter | Returns | Description |
|---|---|---|---|
| 🟢 | `backgroundWidgetVisibility` | `bool` | `false` hides the Background Widget entirely |
| 🟢 | `appCurrentState` | `SystemState` | The current state of the app |
| 🟢 | `backgroundWidgetColor` | `Color` | The current background widget color |

> All getters are 🟢 **Public** — they expose private fields safely.

---

## Methods

### 🟢 `setBackgroundWidgetVisibility()`

```dart
void setBackgroundWidgetVisibility(bool val)
```

Shows or hides the Background Widget. When set to `false`, also unfocuses any active widget (e.g. a focused text field).

| Parameter | Type | Description |
|---|---|---|
| `val` | `bool` | `true` to show, `false` to hide the background widget |

:::info
Unfocusing is handled here intentionally — any call to hide the background widget typically means the user is no longer interacting with a text field.
:::

---

### 🟢 `setAppCurrentState()`

```dart
void setAppCurrentState(SystemState val)
```

Updates the app's current state and automatically adjusts the background widget color to match.

| Parameter | Type | Description |
|---|---|---|
| `val` | `SystemState` | The new app state to transition to |

:::note Color Behavior
| State | Background Color |
|---|---|
| `SystemState.peekAtRoute` | `Colors.transparent` — reveals the map path underneath |
| `SystemState.showSuggestedRoutes` | Semi-transparent white `rgba(255,255,255,0.33)` |
| Any other state | `Colors.white` |
:::

---

### 🔒 `_setBackgroundWidgetColor()`

```dart
void _setBackgroundWidgetColor(Color color)
```

Internal method that updates the background widget color and notifies listeners.

| Parameter | Type | Description |
|---|---|---|
| `color` | `Color` | The new color to apply to the background widget |

:::warning
This is a private method. Use `setAppCurrentState()` instead — it calls this automatically based on the state.
:::

---

## Legend

| Symbol | Meaning |
|---|---|
| 🟢 | Public — accessible from outside the class |
| 🔒 | Private — internal use only (prefixed with `_`) |