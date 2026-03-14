# Map Embedding

MeshMapper provides an embeddable, read-only version of the map that you can place on your own website, forum, or dashboard. The embed is a lightweight view with no controls or popups — just the map, coverage grid, and repeater markers.

## Embed URL

The base URL for any region's embed is:

```
https://[IATA].meshmapper.net/embed.php
```

For example, to embed the **YOW** region:

```
https://yow.meshmapper.net/embed.php
```

## Basic Usage

Add the embed to your site using an HTML `<iframe>`:

```html
<iframe
  src="https://yow.meshmapper.net/embed.php"
  width="100%"
  height="400"
  style="border: none;"
  loading="lazy">
</iframe>
```

## URL Parameters

All parameters are optional and appended as query strings (e.g., `?lat=45.4&lon=-75.7&zoom=14`).

### Map Position

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `lat` | float | Region center | Latitude to center the map on. |
| `lon` | float | Region center | Longitude to center the map on. |
| `zoom` | integer | `12` | Map zoom level (1–19). Ignored if `meters` is set. |
| `meters` | integer | — | Sets the zoom level so that the given number of meters fits across the map width. Overrides `zoom`. |

### Layer Visibility

Toggle layers on or off with `1` (show) or `0` (hide).

| Parameter | Layer | Default |
| --- | --- | --- |
| `cov_grid` | Bidirectional coverage (green) | `1` (on) |
| `part_grid` | TX Only coverage (orange) | `1` (on) |
| `rx_grid` | RX coverage (purple) | `1` (on) |
| `disc_grid` | Discovery coverage (cyan) | `1` (on) |
| `drop_grid` | Dead coverage (grey) | `1` (on) |
| `fail_grid` | Dropped packets (red) | `0` (off) |
| `repeaters` | Repeater markers | `1` (on) |

### Other Options

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `geofence` | `0` or `1` | Auto | Limits loaded data to a 5 km radius around the center. Automatically enabled when `lat`/`lon` are provided. Set `geofence=0` to load all region data instead. |

## Examples

**Center on a specific location at zoom 14:**

```
https://yow.meshmapper.net/embed.php?lat=45.4034&lon=-75.7258&zoom=14
```

**Show a 2 km wide view with only bidirectional coverage:**

```
https://yow.meshmapper.net/embed.php?lat=45.4034&lon=-75.7258&meters=2000&part_grid=0&rx_grid=0&disc_grid=0&drop_grid=0
```

**Load all region data (no geofence) centered on a point:**

```
https://yow.meshmapper.net/embed.php?lat=45.4034&lon=-75.7258&geofence=0
```

## Notes

- The embed is **read-only** — there are no interactive popups, layer toggles, or controls.
- A small "Open on **MeshMapper**" link appears in the bottom-left corner, linking viewers to the full map.
- When `lat` and `lon` are provided, the embed automatically geofences data to a 5 km radius for faster loading. Use `geofence=0` to override this.
