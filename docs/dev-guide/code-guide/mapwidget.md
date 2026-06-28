# MapWidget

> A `StatefulWidget` that renders the MapLibre map. Accepts a `MapWidgetController` to allow external control of the map state.

---

## 📦 Properties

| Symbol | Property | Type | Description |
|---|---|---|---|
| 🟢 | `controller` | `MapWidgetController?` | External controller used to call map functions from outside the widget |

---

---

# MapWidgetController

> A controller class that provides a public interface to interact with the private `_MapWidget` state. 
> 
> The original object is initialized in MapHelperProvider and then passed to the MapWidget constructor. Then upon MapWidget initialization, the pointer towards the
> state widget of the MapWidget will be passed towards the MapHelperProvider object in order for the rest of the app to be able to use accessible methods.

:::tip
Instantiate `MapWidgetController` in a provider or parent widget, pass it to `MapWidget`, then call its methods from anywhere that has access to the controller instance.
:::

---

## Methods

### 🟢 `shortestPath()`

```dart
Future<void> shortestPath(LatLng origin, LatLng dest)
```

Delegates to `_MapWidget.shortestPath()`. Computes and renders the shortest path between two coordinates.

| Parameter | Type | Description |
|---|---|---|
| `origin` | `LatLng` | The starting coordinates |
| `dest` | `LatLng` | The destination coordinates |

:::warning
This method is currently marked as obsolete. Use `drawPath()` with pre-fetched route data instead.
:::

---

### 🟢 `drawPath()`

```dart
Future<void> drawPath(Map<String, dynamic> pathJSON)
```

Delegates to `_MapWidget.drawPath()`. Renders route lines on the map from backend-returned JSON data.

| Parameter | Type | Description |
|---|---|---|
| `pathJSON` | `Map<String, dynamic>` | Route data returned from the backend |

---

### 🟢 `flyToBounds()`

```dart
Future<void> flyToBounds(List<LatLng> bounds)
```

Delegates to `_MapWidget.flyToBounds()`. Animates the camera to fit a list of coordinates within the viewport.

| Parameter | Type | Description |
|---|---|---|
| `bounds` | `List<LatLng>` | List of coordinates to fit in view |

---

### 🟢 `clearLayersAndSources()`

```dart
void clearLayersAndSources()
```

Delegates to `_MapWidget._clearLayersAndSources()`. Removes all route layers and sources from the map.

---

### 🟢 `flyToLoc()`

```dart
Future<void> flyToLoc(LatLng coordinates)
```

Delegates to `_MapWidget._flyToLoc()`. Animates the camera to a specific coordinate at zoom level `15.0`.

| Parameter | Type | Description |
|---|---|---|
| `coordinates` | `LatLng` | Target location to fly to |

---

### 🟢 `addLayers()`

```dart
void addLayers()
```

Delegates to `_MapWidget.addLayers()`. Adds terminal and TODA layers to the map.

---

### 🟢 `addUserMarker()`

```dart
Future<void> addUserMarker(LatLng coords, double rotation)
```

Delegates to `_MapWidget._addUserMarker()`. Places or updates the user's location marker on the map.

| Parameter | Type | Description |
|---|---|---|
| `coords` | `LatLng` | The user's current coordinates |
| `rotation` | `double` | Rotation angle in degrees for the marker icon |

---

### 🟢 `removeMarker()`

```dart
Future<void> removeMarker(String sourceLayerId)
```

Delegates to `_MapWidget._removeMarker()`. Removes a specific marker by its source/layer ID.

| Parameter | Type | Description |
|---|---|---|
| `sourceLayerId` | `String` | The ID used when the marker was created |

---

---

# _MapWidget (Private State)

> The internal `State` class for `MapWidget`. Contains all map logic, rendering, and event handling. Not accessible directly — use `MapWidgetController` instead.

---

## 📦 Properties

| Symbol | Property | Type | Description |
|---|---|---|---|
| 🔒 | `_controller` | `MapLibreMapController?` | The MapLibre map controller, available after `onMapCreated` fires |
| 🔒 | `mapStyle` | `String` | The loaded JSON map style string |
| 🔒 | `routeSourceIds` | `List<String>` | Tracks all GeoJSON source IDs added for route rendering |
| 🔒 | `routeLayerIds` | `List<String>` | Tracks all layer IDs added for route rendering |

---

## Methods

### 🔒 `_loadStyle()`

```dart
Future<void> _loadStyle()
```

Loads the custom map style JSON from `assets/map_styles/osm_bright2.json` and applies it to the map. Based on Stadia Map's OSM Bright style.

---

### 🔒 `_clearLayersAndSources()`

```dart
Future<void> _clearLayersAndSources()
```

Iterates through all tracked layer and source IDs and removes them from the map, then clears both tracking lists. Prevents duplicate layers when re-rendering routes.

---

### 🔒 `_removeMarker()`

```dart
Future<void> _removeMarker(String sourceLayerId)
```

Removes a specific marker's layer and source from the map using the pattern `route-<sourceLayerId>`.

| Parameter | Type | Description |
|---|---|---|
| `sourceLayerId` | `String` | The ID suffix used when the marker was originally added |

---

### 🟢 `drawPath()`

```dart
Future<void> drawPath(Map<String, dynamic> pathJSON)
```

Parses route JSON, clears existing layers, then draws each route segment as a line on the map. Also places green (origin) and red (destination) markers.

| Parameter | Type | Description |
|---|---|---|
| `pathJSON` | `Map<String, dynamic>` | Backend route response parsed into a map |

:::note Line Styles
| Segment Type | Style |
|---|---|
| `walk` | Blue dotted line (`lineDasharray: [1, 1]`) |
| Vehicle | Solid colored line |
:::

---

### 🔒 `_addMarker()`

```dart
Future<void> _addMarker(LatLng coords, String imageId, String markerId)
```

Adds a symbol marker to the map at the given coordinates using a pre-loaded image asset.

| Parameter | Type | Description |
|---|---|---|
| `coords` | `LatLng` | Where to place the marker |
| `imageId` | `String` | The image asset ID (e.g. `mapmarker_green`, `mapmarker_red`) |
| `markerId` | `String` | Unique ID used for the source and layer |

---

### 🔒 `_addUserMarker()`

```dart
Future<void> _addUserMarker(LatLng coords, double rotation)
```

Adds or replaces the user location marker. Removes any existing user marker before placing the new one. Supports rotation for directional heading display.

| Parameter | Type | Description |
|---|---|---|
| `coords` | `LatLng` | The user's current coordinates |
| `rotation` | `double` | Icon rotation in degrees |

:::info
Uses `iconRotationAlignment: "map"` so the marker rotates with the map.
:::

---

### 🔒 `_flyToLoc()`

```dart
Future<void> _flyToLoc(LatLng coordinates)
```

Animates the camera to a specific location at zoom level `15.0`.

| Parameter | Type | Description |
|---|---|---|
| `coordinates` | `LatLng` | Target coordinates to center the camera on |

---

### 🟢 `flyToBounds()`

```dart
Future<void> flyToBounds(List<LatLng> coordinates)
```

Computes the bounding box of a list of coordinates and animates the camera to fit all of them within the viewport with padding.

| Parameter | Type | Description |
|---|---|---|
| `coordinates` | `List<LatLng>` | The set of coordinates to fit in view |

:::note Camera Padding
| Side | Padding |
|---|---|
| Left | `120` |
| Right | `120` |
| Top | `50` |
| Bottom | `250` |
:::

---

### 🟢 `addLayers()`

```dart
void addLayers()
```

Entry point that calls both `addTerminalLayers()` and `addTodaLayers()` to populate the map with terminal and TODA icons.

:::warning
Currently unused pending a decision on whether to display jeepney and tricycle terminals.
:::

---

### 🟢 `addTodaLayers()`

```dart
Future<void> addTodaLayers()
```

Fetches the TODA list from `DatabaseService` and adds a symbol layer for each entry using the `toda` icon. Visible from zoom level `12`.

---

### 🟢 `addTerminalLayers()`

```dart
Future<void> addTerminalLayers()
```

Fetches the terminal list from `DatabaseService` and adds a symbol layer for each entry using the `bus` icon. Visible from zoom level `8`.

---

### 🟢 `shortestPath()`

```dart
Future<void> shortestPath(LatLng origin, LatLng dest)
```

Directly calls the backend API to fetch and render the shortest path between two points.

| Parameter | Type | Description |
|---|---|---|
| `origin` | `LatLng` | Starting coordinates |
| `dest` | `LatLng` | Destination coordinates |

:::danger Obsolete
This function is no longer the recommended approach. Route fetching and rendering have been separated — fetch routes via `queryForShortestPath()` and render via `drawPath()` instead.
:::

---

### 🟢 `determinePosition()`

```dart
Future<Position> determinePosition()
```

Checks location service availability and permissions, then returns the device's current GPS position.

:::note Permission Flow
1. Checks if location services are enabled
2. Checks current permission status
3. Requests permission if denied
4. Returns error if permanently denied
5. Returns current position if all checks pass
:::

---

### 🔒 `_addImageToController()`

```dart
Future<void> _addImageToController(
  String imgDirectory,
  String imgID,
  bool symbolIconAllowOverlap,
)
```

Loads an image asset from the bundle and registers it with the MapLibre controller for use as a symbol icon.

| Parameter | Type | Description |
|---|---|---|
| `imgDirectory` | `String` | Asset path (e.g. `assets/img/toda.png`) |
| `imgID` | `String` | The ID to reference this image in layer properties |
| `symbolIconAllowOverlap` | `bool` | Whether the icon can overlap other symbols |

---

## Legend

| Symbol | Meaning |
|---|---|
| 🟢 | Public — accessible from outside the class |
| 🔒 | Private — internal use only (prefixed with `_`) |