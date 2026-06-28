# MapHelperProvider

> Extends `ChangeNotifier` — handles map-related state and utilities, including movement direction calculation based on position history.

---

## 📦 Properties

| Symbol | Property | Type | Default | Description |
|---|---|---|---|---|
| 🟢 | `mapWidgetController` | `MapWidgetController` | `MapWidgetController()` | Controller for the MapLibre map widget |
| 🔒 | `_position1` | `LatLng` | `LatLng(-90, -180)` | The most recent recorded position |
| 🔒 | `_position2` | `LatLng` | `LatLng(-90, -180)` | The second most recent recorded position |

:::info Sentinel Values
`LatLng(-90, -180)` is used as a **null substitute** to indicate that no real position has been recorded yet. A longitude of `-180` inside the Philippines is geographically impossible, making it a safe sentinel.
:::

---

## Methods

### 🟢 `shiftPosition()`

```dart
void shiftPosition(LatLng pos)
```

Updates position history by shifting the current position to `_position2` and saving the new position as `_position1`. Call this every time the user's location updates.

| Parameter | Type | Description |
|---|---|---|
| `pos` | `LatLng` | The latest GPS position |

:::note
At least **two calls** to `shiftPosition()` are required before `getMovementDirectionFromYourPositionHistory()` can return a meaningful direction.
:::

---

### 🟢 `getMovementDirectionFromYourPositionHistory()`

```dart
double getMovementDirectionFromYourPositionHistory()
```

Computes the compass bearing (in degrees) based on the two most recently recorded positions using the **forward azimuth formula**.

**Returns:** `double` — a bearing in degrees (`0°–360°`), where `0°` is North.

| Degrees | Direction |
|---|---|
| `0°` | North |
| `90°` | East |
| `180°` | South |
| `270°` | West |

:::note Calculation
Uses the forward azimuth (bearing) formula:

1. Converts lat/lng to radians
2. Computes `y = sin(Δlng) × cos(lat2)`
3. Computes `x = cos(lat1) × sin(lat2) − sin(lat1) × cos(lat2) × cos(Δlng)`
4. Returns `(atan2(y, x) × 180/π + 360) % 360` to normalize to `0°–360°`
:::

:::warning
Returns `0.0` early if `_position2` is still the sentinel value `LatLng(-90, -180)`, meaning not enough position history has been recorded yet.
:::

---

### 🟢 `resetPosValues()`

```dart
void resetPosValues()
```

Resets both `_position1` and `_position2` back to their sentinel values `LatLng(-90, -180)`, effectively clearing position history.

:::tip
Call this when the user starts a new navigation session or when stale position history could produce incorrect bearing results.
:::

---

## Legend

| Symbol | Meaning |
|---|---|
| 🟢 | Public — accessible from outside the class |
| 🔒 | Private — internal use only (prefixed with `_`) |