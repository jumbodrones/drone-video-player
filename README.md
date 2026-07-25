# Drone Video Viewer

A browser-based tool for exploring DJI drone footage with synchronized GPS telemetry, live flight maps, altitude/speed charts, and geographic feature tagging. No installation or server required — open the HTML file in any modern browser.

Built for use in drone data capture and analysis courses alongside GIS, remote sensing, and photogrammetry workflows.

---

## Features

- **Synchronized telemetry** — altitude, speed, heading, satellite count, and GPS coordinates update in real time as video plays
- **Live flight map** — altitude-colored flight path, animated drone icon, camera heading cone, and home point marker (Leaflet.js)
- **Telemetry chart** — full-flight altitude and speed profile; click to seek, hover for precise values
- **Feature tagging** — tag points by drone position (`T`) or by clicking a video pixel (pixel-to-geographic projection)
- **CSV export** — download telemetry frames or tagged features for use in Python, QGIS, or ArcGIS
- **Orthomosaic overlay** — load a GeoTIFF or PNG/JPG + world file to overlay a processed map product on the flight path
- **Multi-format SRT parsing** — supports Phantom, Mavic, Mini, and Air series DJI firmware formats

---

## Quick Start

1. Open `drone-video-viewer.html` in Chrome or Firefox
2. Click **📂 Load Files** and select your `.mp4` and `.srt` files together
3. The map populates, the HUD appears, and telemetry syncs to playback

**No drone footage yet?** Sample files are included in this repository — load `sample-mapping-flight.mp4` and `sample-mapping-flight.srt` to explore a real flight right away.

No internet connection is required after the page loads (map tiles require connectivity).

---

## File Requirements

| File | Description |
|------|-------------|
| `.mp4` | DJI video file |
| `.srt` | DJI telemetry subtitle file (recorded alongside the video) |
| `.tif` / `.tiff` | GeoTIFF orthomosaic (optional, WGS84 / EPSG:4326) |
| `.pgw` / `.jgw` / `.tfw` | World file for non-georeferenced rasters (optional) |

---

## Usage Notes

- **Orthomosaics** must be in WGS84 (EPSG:4326). Reproject in QGIS if needed: *Raster → Projections → Warp (Reproject)*
- **Pixel projection** accuracy assumes nadir camera angle and flat terrain at drone altitude
- **CSV telemetry export** columns: `time_s`, `latitude`, `longitude`, `alt_rel_m`, `alt_abs_m`, `h_speed_ms`, `v_speed_ms`, `satellites`
- **Tags CSV export** columns: `id`, `time_s`, `time_hms`, `latitude`, `longitude`, `altitude_m`, `label`

Full usage documentation is in [`USER_GUIDE.md`](USER_GUIDE.md).

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `←` / `→` | Skip ±10 seconds |
| `M` | Toggle mute |
| `T` | Tag at current drone position |
| `C` | Toggle click-to-tag mode |

---

## Classroom Use

- Students load their own flight files with no software installation
- Telemetry CSV imports directly into pandas/GeoPandas, QGIS, or ArcGIS
- Tag CSV imports as a point layer in QGIS: *Layer → Add Layer → Add Delimited Text Layer*
- Pair with an orthomosaic overlay to compare raw video to the processed map product

---

## Tech Stack

- Vanilla HTML/CSS/JavaScript — single file, zero dependencies to install
- [Leaflet.js](https://leafletjs.com/) for mapping
- OpenStreetMap and Esri World Imagery tile layers

---

## License

For educational use. Contact Jon Caris (Jon.Caris@tufts.edu) for questions.
