# SearchDetailsProvider

> Extends `ChangeNotifier` — manages search field state, location selection, and route computation.

---

## 📦 Properties

| Symbol | Property | Type | Description |
|---|---|---|---|
| 🟢 | `isLoading` | `bool` | Indicates if a loading operation is in progress |
| 🟢 | `fromLocResults` | `List<NominatimPlace>` | Search results for the From location field |
| 🟢 | `toLocResults` | `List<NominatimPlace>` | Search results for the To location field |
| 🟢 | `fromLocController` | `TextEditingController` | Controls the From location text field |
| 🟢 | `toLocController` | `TextEditingController` | Controls the To location text field |
| 🟢 | `toLocFocusNode` | `FocusNode` | Focus node for the To location text field |
| 🔒 | `_isFromLocTextfieldEmpty` | `bool` | Tracks if the From text field is empty |
| 🔒 | `_isToLocTextfieldEmpty` | `bool` | Tracks if the To text field is empty |
| 🔒 | `_selectedFromLocationDetails` | `LatLng?` | Stores the selected From location coordinates |
| 🔒 | `_selectedToLocationDetails` | `LatLng?` | Stores the selected To location coordinates |
| 🔒 | `_userCurrentGeoLoc` | `LatLng` | The user's current GPS coordinates (refreshed every 10s) |
| 🔒 | `_suggestedShortestPaths` | `Map<String, dynamic>` | Raw route data returned from the backend |

> 🟢 **Public** · 🔒 **Private**

---

## 🔍 Getters

| Symbol | Getter | Returns | Description |
|---|---|---|---|
| 🟢 | `isFromLocationDetailsEmpty` | `bool` | `true` if a From location has been selected |
| 🟢 | `isToLocationDetailsEmpty` | `bool` | `true` if a To location has been selected |
| 🟢 | `isFromLocTextfieldEmpty` | `bool` | `true` if the From text field is empty |
| 🟢 | `isToLocTextfieldEmpty` | `bool` | `true` if the To text field is empty |
| 🟢 | `selectedFromLocationDetails` | `LatLng?` | The selected From location coordinates |
| 🟢 | `selectedToLocationDetails` | `LatLng?` | The selected To location coordinates |
| 🟢 | `getUserCurrentGeoLoc` | `LatLng` | The user's current GPS coordinates |
| 🟢 | `suggestedShortestPaths` | `Map<String, dynamic>` | The computed shortest path results from the backend |

> All getters are 🟢 **Public** — they expose private fields safely.

---

## Methods

### 🟢 `setTextfieldEmptyStatus()`

```dart
void setTextfieldEmptyStatus(bool val, SearchFieldType type)
```

Tracks whether the From or To text field is empty. Used by background widgets to decide whether to show the results `ListView`.

| Parameter | Type | Description |
|---|---|---|
| `val` | `bool` | The empty status to set |
| `type` | `SearchFieldType` | Target field — `from` or `to` |

---

### 🟢 `tryToEraseLocResults()`

```dart
void tryToEraseLocResults(SearchFieldType type)
```

Clears existing search results when the user starts typing again.

| Parameter | Type | Description |
|---|---|---|
| `type` | `SearchFieldType` | Target field — `from` or `to` |

---

### 🟢 `saveLocSearchResults()`

```dart
void saveLocSearchResults(List<NominatimPlace> resultList, SearchFieldType type)
```

Saves Nominatim search results into the provider. Triggers a rebuild of the result list widgets via `notifyListeners()`.

| Parameter | Type | Description |
|---|---|---|
| `resultList` | `List<NominatimPlace>` | The list of place results to save |
| `type` | `SearchFieldType` | Target field — `from` or `to` |

---

### 🟢 `getUserCurrentLocAndSaveToContext()`

```dart
Future<void> getUserCurrentLocAndSaveToContext()
```

Uses the `Geolocator` library to fetch the user's current GPS position and stores it as a `LatLng`.

:::info
Uses `LocationAccuracy.best` for highest precision.
:::

---

### 🟢 `useCurrentUserGeoLocAsOrigin()`

```dart
void useCurrentUserGeoLocAsOrigin(BuildContext context)
```

Sets the user's current GPS location as the From (origin) location.

---

### 🟢 `setFromLocationDetails()`

```dart
void setFromLocationDetails(NominatimPlace nomiDetails)
```

Public wrapper that sets the From location using a `NominatimPlace` object. Delegates to `_setFromLocationDetails()`.

| Parameter | Type | Description |
|---|---|---|
| `nomiDetails` | `NominatimPlace` | The selected place from search results |

---

### 🔒 `_setFromLocationDetails()`

```dart
void _setFromLocationDetails(double lat, double lon, String name)
```

Internal implementation that applies the From location. Updates the text field, coordinates, and shifts focus to the To field.

| Parameter | Type | Description |
|---|---|---|
| `lat` | `double` | Latitude of the selected location |
| `lon` | `double` | Longitude of the selected location |
| `name` | `String` | Display name for the text field |

---

### 🟢 `setToLocationDetails_andStartCalculating()`

```dart
void setToLocationDetails_andStartCalculating(
  NominatimPlace nomiDetails,
  BuildContext buildContext,
)
```

Public wrapper that sets the To location and triggers route computation. Delegates to `_setToLocationDetails_andStartCalculating()`.

| Parameter | Type | Description |
|---|---|---|
| `nomiDetails` | `NominatimPlace` | The selected destination place |
| `buildContext` | `BuildContext` | Used to read providers from context |

---

### 🔒 `_setToLocationDetails_andStartCalculating()`

```dart
void _setToLocationDetails_andStartCalculating(
  double lat,
  double lon,
  String name,
  BuildContext buildContext,
)
```

Internal implementation that sets the To location and kicks off shortest path computation.

| Parameter | Type | Description |
|---|---|---|
| `lat` | `double` | Latitude of the destination |
| `lon` | `double` | Longitude of the destination |
| `name` | `String` | Display name for the text field |
| `buildContext` | `BuildContext` | Used to read `SystemVariablesProvider` and `SearchDetailsProvider` |

:::note Flow
1. Clears previous suggested paths
2. Updates To location text field
3. Unfocuses keyboard after a 200ms delay
4. Sets app state to `waitingForBackendResponse`
5. Queries backend for shortest path
6. Sets app state to `showSuggestedRoutes`
:::

---

### 🟢 `wipeSuggestedShortestPaths()`

```dart
void wipeSuggestedShortestPaths()
```

Clears all previously computed shortest path data from the provider.

---

### 🟢 `saveSuggestedShortestPaths()`

```dart
void saveSuggestedShortestPaths(Map<String, dynamic> val)
```

Stores the backend-returned shortest path data into the provider.

| Parameter | Type | Description |
|---|---|---|
| `val` | `Map<String, dynamic>` | The route data returned from the backend |

---

### 🟢 `getRouteByID()`

```dart
Map<String, dynamic> getRouteByID(String route_id)
```

Retrieves a single route from the suggested paths by its ID.

| Parameter | Type | Description |
|---|---|---|
| `route_id` | `String` | Route identifier in format `result-<number>` (e.g. `result-1`) |

**Returns:** `Map<String, dynamic>` — a filtered map containing only the requested route.

:::tip 
Valid ID Format
`result-1`, `result-2`, `result-3`, ...
:::