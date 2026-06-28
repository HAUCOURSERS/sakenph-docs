# SystemTasksProvider

> Extends `ChangeNotifier` — manages repeating background tasks such as GPS position streaming. Depends on other providers that must be mounted before use.

:::warning
`mountProviders()` **must be called before** `start_repeatingTask()`, otherwise the task will not run.
:::

---

## 📦 Properties

| Symbol | Property | Type | Default | Description |
|---|---|---|---|---|
| 🔒 | `_isRunning_repeatingTask` | `bool` | `false` | Tracks whether the repeating task is currently active |
| 🔒 | `_systemVariablesProvider` | `SystemVariablesProvider?` | `null` | Injected dependency for app state |
| 🔒 | `_searchDetailsProvider` | `SearchDetailsProvider?` | `null` | Injected dependency for search state |
| 🔒 | `_mapHelperProvider` | `MapHelperProvider?` | `null` | Injected dependency for map operations |
| 🔒 | `_positionStream` | `StreamSubscription<Position>?` | `null` | The active GPS position stream subscription |

---

## Methods

### 🟢 `mountProviders()`

```dart
void mountProviders(BuildContext context)
```

Reads and stores references to the required providers from the widget tree. Must be called once before starting any repeating tasks.

| Parameter | Type | Description |
|---|---|---|
| `context` | `BuildContext` | The widget context used to read providers |

:::info
Internally reads `SystemVariablesProvider`, `SearchDetailsProvider`, and `MapHelperProvider` via `context.read()`.
:::

---

### 🟢 `start_repeatingTask()`

```dart
void start_repeatingTask()
```

Starts the GPS position stream. Checks that all provider dependencies are mounted before proceeding.

:::note Flow
1. Calls `_checkDependencies()` — aborts if any provider is `null`
2. Calls `_startStream()` to begin listening to GPS updates
:::

---

### 🟢 `stop_repeatingTask()`

```dart
void stop_repeatingTask()
```

Stops the active GPS position stream and cleans up the subscription.

---

### 🔒 `_checkDependencies()`

```dart
bool _checkDependencies()
```

Validates that all required provider dependencies have been mounted.

**Returns:** `true` if all providers are non-null, `false` otherwise.

---

### 🔒 `_startStream()`

```dart
void _startStream()
```

Requests GPS permission and starts listening to the device's position stream using `Geolocator`.

:::note Stream Settings
| Setting | Value |
|---|---|
| Accuracy | `LocationAccuracy.high` |
| Distance Filter | `0` meters (update on every position change) |
:::

:::warning
If location permission is not granted, the stream will not start.
:::

---

### 🔒 `_stopStream()`

```dart
void _stopStream()
```

Cancels the active `StreamSubscription` and sets it to `null`.

:::danger
Will throw a null error if called before `_startStream()` has been invoked. Always call `stop_repeatingTask()` instead, which handles this safely.
:::

---

## Legend

| Symbol | Meaning |
|---|---|
| 🟢 | Public — accessible from outside the class |
| 🔒 | Private — internal use only (prefixed with `_`) |