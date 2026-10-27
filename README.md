# Drone Video Viewer

A browser-based tool for exploring DJI drone footage with synchronized GPS telemetry: live flight maps, camera footprints, a 3D flight view, geotagged frame grabs, and a field-observation / inspection report builder. No installation or server required — open the HTML file in any modern browser.

Built for use in drone data capture and analysis courses alongside GIS, remote sensing, and photogrammetry workflows.

---

## Features

**Playback & telemetry**
- **Synchronized telemetry** — altitude, speed, heading, satellite count, and GPS coordinates update in real time as video plays
- **Live flight map** — altitude-colored flight path, animated drone icon, camera heading cone, and home point marker (Leaflet.js); satellite or street basemap
- **Telemetry chart** — full-flight altitude and speed profile; click to seek, hover for precise values
- **Multi-format SRT parsing** — Phantom, Mavic, Mini, Air and Matrice firmware formats, including the timestamp line used for photo EXIF

**Camera geometry**
- **Camera footprint** — the video frame projected onto the ground as it plays, with view-cone rays, a coverage trail (or the whole flight at once), oblique views clipped at a max range, and a live readout of footprint size and ground sample distance
- **Calibration** — drone model presets for field of view, plus pitch and heading overrides for SRTs that don't record gimbal angles
- **3D flight view** — orbitable 3D track colored by altitude, with ground shadow, curtain lines, live drone position and camera footprint; vertical exaggeration, top/side views, click the track to jump the video
- **Altitude profile** — altitude vs. distance along track, click to seek

**Capture & export**
- **📸 Capture tray** — capture frames while the video plays; thumbnails collect under the video, ready for the report
- **Geotagged frame grabs** — download the current frame as a JPG with EXIF GPS (position, altitude, camera direction, time) and DJI-style XMP gimbal angles; optional telemetry overlay with compass and scale bar
- **Batch extraction** — save a frame every N seconds as a ZIP with `frames.csv` and an OpenDroneMap / WebODM `geo.txt` for video-based photogrammetry
- **CSV export** — download telemetry frames or tagged features for use in Python, QGIS, or ArcGIS

**Inspection report**
- **Report builder** — one row per captured frame with location data, camera pitch (side view) and yaw (top view) diagrams, ground footprint, and notes
- **Annotate** — draw arrows, boxes, ellipses, freehand and text labels; measure approximate ground distances by clicking two points
- **Per-frame camera tilt** — type the tilt or click the horizon line in the image to calculate it (for SRTs without gimbal pitch)
- **Overview** — automatic flight summary, satellite map with numbered observations and their camera field-of-view cones, and an area description drafted from OpenStreetMap
- **Print / Save PDF** — clean light-themed report layout; work in progress autosaves in the browser

**Other**
- **Feature tagging** — tag points by drone position (`T`) or by clicking a video pixel (pixel-to-geographic projection)
- **Orthomosaic overlay** — load a GeoTIFF or PNG/JPG + world file to overlay a processed map product on the flight path

---

## Quick Start

1. Open `drone-video-viewer.html` in Chrome, Edge, Firefox or Safari
2. Click **📂 Load Files** and select your `.mp4` and `.srt` files together
3. The map populates, the HUD appears, and telemetry syncs to playback

### Making an inspection report

1. Play the video and press **📸** (or `G`) whenever you see something worth recording — no need to pause
2. Click **📝 Report →** at the end of the Captures tray
3. Pick your drone with **🚁** in the top bar (or **Drone / camera** at the top of the report) — this sets the field of view
4. For each frame: set the camera **Tilt** if the SRT has no gimbal pitch (type it or **⌖ Click horizon**), add notes, draw, and measure
5. Click **↻ Describe from flight location** for an area description draft, then edit
6. Click **🖨 Print / Save PDF** and choose **Save as PDF**

---

## File Requirements

| File | Description |
|------|-------------|
| `.mp4` / `.mov` | DJI video file |
| `.srt` | DJI telemetry subtitle file (recorded alongside the video) |
| `.csv` | Flight-log CSV from AirData, Flight Reader or PhantomHelp (optional — sets the true home point) |
| `.tif` / `.tiff` | GeoTIFF orthomosaic (optional, WGS84 / EPSG:4326) |
| `.pgw` / `.jgw` / `.tfw` | World file for non-georeferenced rasters (optional) |

DJI `FlightRecord_*.txt` files are encrypted and can't be read in the browser — export them to CSV first.

---

## Usage Notes

- **Gimbal angles:** many current DJI SRTs (Mavic 3, Air 3, Mini 4 and others) don't record gimbal pitch, so the camera is assumed to point straight down until you set the tilt. Footprints, cones and measurements depend on it.
- **Measurements and footprints** are flat-ground estimates from the camera model — accuracy drops for oblique views, slopes and tall objects.
- **Orthomosaics** must be in WGS84 (EPSG:4326). Reproject in QGIS if needed: *Raster → Projections → Warp (Reproject)*
- **Internet needed for:** map tiles, the report's satellite map, and the OpenStreetMap area description. Everything else works offline once the page has loaded.
- **Report autosave** is stored in the browser on that computer only — print to PDF to keep a permanent copy.
- **CSV telemetry export** columns: `time_s`, `latitude`, `longitude`, `alt_rel_m`, `alt_abs_m`, `h_speed_ms`, `v_speed_ms`, `satellites`
- **Tags CSV export** columns: `id`, `time_s`, `time_hms`, `latitude`, `longitude`, `altitude_m`, `label`

Full usage documentation is in [`USER_GUIDE.md`](USER_GUIDE.md). Publishing instructions are in [`HOSTING.md`](HOSTING.md).

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `←` / `→` | Skip ±10 seconds |
| `M` | Toggle mute |
| `G` or `R` | Capture current frame for the report |
| `T` | Tag at current drone position |
| `C` | Toggle click-to-tag mode |
| `3` | Switch between map and 3D view |
| `Esc` | Stop the current drawing tool / close the report |

---

## Classroom Use

- Students load their own flight files with no software installation — files never leave their computer
- The inspection report turns a flight video into a structured field-observation assignment: pick frames, annotate and measure features, describe the site, and submit the PDF to Canvas
- Telemetry CSV imports directly into pandas/GeoPandas, QGIS, or ArcGIS
- Tag CSV imports as a point layer in QGIS: *Layer → Add Layer → Add Delimited Text Layer*
- Geotagged frame grabs drop straight onto a map in QGIS, ArcGIS Pro or Google Earth
- Use the coverage trail and 3D view to show how altitude and gimbal pitch change what the camera sees, then extract frames and run them through WebODM
- Pair with an orthomosaic overlay to compare raw video to the processed map product

---

## Tech Stack

- Vanilla HTML/CSS/JavaScript — single file, zero dependencies to install
- [Leaflet.js](https://leafletjs.com/) for mapping; geotiff.js loaded on demand for GeoTIFF overlays
- Esri World Street Map and World Imagery tile layers
- OpenStreetMap Nominatim and Overpass APIs for the report's area description
- Built-in EXIF/XMP writer, ZIP writer and 3D renderer (no extra libraries)

---

## License

For educational use. Contact Jon Caris (Jon.Caris@tufts.edu) for questions.
