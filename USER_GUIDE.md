# Drone Video Viewer — User Guide

A browser-based tool for exploring DJI drone footage with synchronized GPS telemetry, live flight maps, altitude/speed charts, and geographic feature tagging.

---

## Getting Started

Open `drone-video-viewer.html` in any modern browser (Chrome or Firefox recommended). No installation or internet connection required after the page loads.

### Loading Your Files

The viewer accepts a DJI video file (`.mp4`) and its matching SRT telemetry file (`.srt`). These are recorded together automatically by DJI drones.

**Don't have drone footage yet?** Sample files (`sample-mapping-flight.mp4` and `sample-mapping-flight.srt`) are included in this repository. Load them the same way as your own files to explore a real mapping flight.

**Option 1 — Load Files button (recommended):** Click **📂 Load Files** in the header and select your `.mp4` and `.srt` together. Both files can be selected at once in the file picker.

**Option 2 — Drag and drop:** Drag your video and SRT files onto the drop zone in the left panel. Both files can be dropped simultaneously.

**Option 3 — Separate loaders:** Use **📹 Video only** or **📄 SRT only** if you need to load them independently.

Once loaded, the map will populate with the flight path, the HUD overlay will appear on the video, and the telemetry strip will become visible at the bottom.

---

## Interface Overview

The viewer is divided into two main panels: **video** on the left and **map** on the right.

### Video Panel

The video plays back your drone footage. A HUD overlay in the upper-left corner shows real-time altitude, speed, and distance from home. Below the video are playback controls.

**Playback controls:**
- **▶ / ⏸** — Play or pause
- **⏪ / ⏩** — Skip backward or forward 10 seconds
- **🔊** — Toggle mute
- **Speed selector** — Set playback rate (0.5×, 1×, 1.5×, 2×)
- **Progress bar** — Click or drag to scrub to any point in the flight

**Keyboard shortcuts:**

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `←` | Skip back 10 s |
| `→` | Skip forward 10 s |
| `M` | Toggle mute |
| `T` | Tag feature at current drone position |
| `C` | Toggle click-to-tag mode |

### Telemetry Strip

A row at the bottom of the screen displays real-time values synchronized to the video: latitude, longitude, relative altitude, absolute altitude, horizontal speed, vertical speed, satellite count, and heading.

### Map Panel

The map shows the drone's flight path as a color-coded line (blue = low altitude, red = high altitude). The drone icon moves in real time as the video plays, with an arrow indicating flight direction and a cone indicating camera heading when available. A green **H** marker shows the home point.

**Map toolbar buttons:**
- **⊕ Re-center** — Pan the map to the current drone position
- **🛰 Satellite / 🗺 Street** — Toggle between satellite imagery and street map
- **🖼 Ortho** — Open the orthomosaic overlay panel
- **🏷 Tags** — Show or hide the tagged features list

---

## Telemetry Chart

Click **📊 Charts** in the header to open the altitude and speed chart below the map. The chart shows the full flight profile with altitude (blue) and horizontal speed (orange). A dashed playhead tracks the current video position.

- **Hover** over the chart to see precise values at any moment
- **Click** the chart to jump the video to that point in the flight

---

## Exporting Telemetry

Click **⬇ CSV** in the header to download a CSV file of all telemetry frames. Columns include `time_s`, `latitude`, `longitude`, `alt_rel_m`, `alt_abs_m`, `h_speed_ms`, `v_speed_ms`, and `satellites`. This file is ready for import into GIS software, spreadsheets, or Python.

---

## Feature Tagging

Feature tags let you mark points of interest in the video and pin them to the map with geographic coordinates.

### Method 1 — Tag at drone position

Press **T** or click the **🏷** button in the playback controls. A dialog appears showing the current timestamp and drone GPS coordinates. Enter a label (e.g., *Erosion*, *Structure*, *Downed tree*) and press **Enter** or click **📍 Tag**.

### Method 2 — Click on video pixels (pixel projection)

This mode translates a specific pixel in the video frame to a geographic coordinate on the map, using the drone's altitude and heading to project through the camera's field of view.

1. Click **🎯** in the playback controls (or press `C`) to enter click-to-tag mode. The button highlights orange.
2. Set the **HFoV** field to your camera's horizontal field of view in degrees. Common values:

   | Drone model | HFoV |
   |-------------|------|
   | DJI Air 3 | 73° |
   | DJI Mini 3 Pro | 83° |
   | DJI Mavic 3 | 84° |
   | DJI Mini 2 | 83° |

3. Hover over the video — an orange crosshair appears on the map showing where your cursor currently maps geographically.
4. Click any pixel to open the tag dialog. The projected coordinates are pre-filled. Enter a label and confirm.

> **Accuracy note:** Pixel projection assumes the camera is pointing straight down (nadir) and the terrain is flat at the drone's altitude. Accuracy is best during nadir mapping flights at consistent altitude. Results will drift on oblique angles or varied terrain.

### Managing Tags

- Tagged features appear as numbered orange circles on the map. Click a marker to see its label, coordinates, and a **▶ Jump to this moment** link.
- Click **🏷 Tags** in the map toolbar to open the tag list. Click any row to seek the video to that moment.
- Click **✕** in the tag list to remove a tag.
- Click **⬇ CSV** in the tag list panel to export all tags as a CSV file with columns: `id`, `time_s`, `time_hms`, `latitude`, `longitude`, `altitude_m`, `label`.

---

## Orthomosaic Overlay

If you have processed your flight into an orthomosaic (georeferenced aerial image), you can overlay it on the map.

1. Click **🖼 Ortho** in the map toolbar to open the overlay panel.
2. Click **📁 Image** to select your orthomosaic file. Supported formats: `.png`, `.jpg`, `.jpeg`, `.tif`, `.tiff`.
3. Set the geographic bounds (N, S, E, W in decimal degrees). There are two ways to fill these:
   - **📐 World file** — Select a `.pgw`, `.jgw`, or `.tfw` world file to fill bounds automatically.
   - **GeoTIFF** — If your image is a GeoTIFF with embedded georeferencing, bounds are detected automatically when the file is loaded.
   - **📍 Flight** — Fills bounds from the loaded flight path (add padding manually if the ortho extends beyond the track).
   - **🗺 Map view** — Fills bounds from the current map viewport.
4. Adjust **Opacity** with the slider.
5. Click **Load** to display the orthomosaic on the map.

> **Large GeoTIFFs:** Files over 500 MB may take a moment to render. The viewer downsamples them to fit within browser canvas limits. For best performance, export your orthomosaic as a `.png` or `.jpg` with a matching world file.

> **Coordinate system:** The orthomosaic must be in WGS84 (EPSG:4326). If yours is in UTM or another projected CRS, reproject it in QGIS first: *Raster → Projections → Warp (Reproject)*.

---

## Supported SRT Formats

The viewer parses multiple DJI firmware SRT formats automatically, including fields for latitude, longitude, relative altitude, absolute altitude, horizontal and vertical speed, satellite count, gimbal pitch/yaw, drone yaw, and compass heading. Formats from the Phantom, Mavic, Mini, and Air series are supported.

If your SRT file fails to parse (badge turns red), check that it is the `.srt` file generated by the drone — not a subtitle file from another source.

---

## Tips for Classroom Use

- Students can load their own flight files without any software installation — just a browser.
- The CSV export is ready for analysis in Python (pandas/GeoPandas), QGIS, or ArcGIS.
- Feature tags export as a CSV that can be imported directly into QGIS as a point layer (*Layer → Add Layer → Add Delimited Text Layer*).
- The pixel projection tool works best during nadir mapping passes at a consistent altitude — ideal for demonstrating the relationship between camera geometry, altitude, and ground resolution.
- Pair the viewer with the orthomosaic overlay to compare raw video to the processed map product from the same flight.
