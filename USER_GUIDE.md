# Drone Video Viewer — User Guide

A browser-based tool for exploring DJI drone footage with synchronized GPS telemetry, live flight maps, altitude/speed charts, and geographic feature tagging.

---

## Getting Started

Open `drone-video-viewer.html` in any modern browser (Chrome or Firefox recommended). No installation or internet connection required after the page loads.

### Loading Your Files

The viewer accepts a DJI video file (`.mp4`) and its matching SRT telemetry file (`.srt`). These are recorded together automatically by DJI drones.

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
- **📸** — Capture the current frame into the report tray (see [Inspection Report](#inspection-report)); works while the video is playing
- **▾** (next to 📸) — Download options: geotagged JPG, JPG with telemetry overlay, batch extraction (see [Frame Grabs](#frame-grabs))
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
| `G` or `R` | Capture current frame for the report |
| `3` | Switch between map and 3D view |

### Telemetry Strip

A row at the bottom of the screen displays real-time values synchronized to the video: latitude, longitude, relative altitude, absolute altitude, horizontal speed, vertical speed, satellite count, and heading.

### Map Panel

The map shows the drone's flight path as a color-coded line (blue = low altitude, red = high altitude). The drone icon moves in real time as the video plays. A **solid arrow** shows the direction the drone's nose is facing (from compass data in the SRT); a **hollow arrow** means the SRT has no compass data, so it shows the GPS direction of travel instead — a drone flying sideways or backwards will face a different way. A yellow cone shows the camera view direction when gimbal yaw is available. A small legend in the lower-left corner of the map explains which one you're seeing.

A green **H** marks the home (takeoff) point when it's known — from a `HOME(...)` field in the SRT, a flight-log CSV, or set by hand. Most current DJI SRT files don't record the home point, so the app falls back to an amber dashed **S** at the spot where the video *started recording*, and the HUD shows **Dist (start)** instead of **Dist (home)**. To set the real home point: drag the marker, click **🏠 Set Home** and then click the map, or load a flight-log CSV exported from AirData, Flight Reader or phantomhelp.com/LogViewer. DJI `FlightRecord_*.txt` files are encrypted and can't be read directly in the browser.

**Map toolbar buttons:**
- **⊕ Re-center** — Pan the map to the current drone position
- **🛰 Satellite / 🗺 Street** — Toggle between satellite imagery and street map
- **🖼 Ortho** — Open the orthomosaic overlay panel
- **🏷 Tags** — Show or hide the tagged features list
- **🧊 3D / 🗺 Map** — Swap the map for the 3D flight view (see [3D Flight View](#3d-flight-view--altitude-profile))
- **📐 Footprint** — Draw the camera's ground footprint (see [Camera Footprint](#camera-footprint))

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

## Inspection Report

Build a field-observation / inspection report from frames in the video, then print it or save it as a PDF.

**1. Watch and capture.** Play the video and press **📸** (or `G`) whenever you see something worth recording. You can capture while the video plays, so there's no need to pause. Each capture appears as a numbered thumbnail in the **Captures** tray under the video, with its telemetry stored alongside it.
- Click a thumbnail to jump the video back to that moment; hover and click **×** to remove it.
- A yellow dot on a thumbnail means it already has notes or markup.
- Pressing 📸 twice on the same frame won't add a duplicate.

When you're done watching, click **📝 Report →** at the end of the tray (or **📝 Report** in the header). All your captures are there ready for annotation. **◀ Video** returns to the player, where you can keep capturing; new frames are added to the end of the report.

**2. Fill in the header.**
- **Title**, **Prepared by** and **Date** (the date is filled in from the SRT when it has timestamps).
- **Drone / camera** and **HFoV °** set the camera's field of view. Pick your drone first; everything that depends on the lens updates when you change it (footprints, map cones, ⌖ Click horizon, measurements). It's the same setting as the **🚁** drone picker in the top bar of the main screen (always visible) and the Model / HFoV controls under the video. If your drone isn't listed, type its horizontal FOV for video mode.
- **Flight summary** is generated automatically: date, recording time, duration, distance flown, altitude range, extent, centre and home point, file names, the **camera field of view** (horizontal × vertical, with the drone model if you picked one), where the camera angles came from (SRT gimbal data or assumed), and the number of observations.
- **Map** shows the track on satellite imagery with numbered pins for each observation and its **camera field of view**: dashed rays from the drone and an orange footprint on the ground showing what that frame covers. A dashed far edge means the view continues to the horizon; on the overview map those cones are cut at about 60% of the flight's size so the track stays readable. Footprints follow the HFoV / Pitch / Hdg± settings under the video, so set those first.
- **Area description:** click **↻ Describe from flight location** for a starting draft. It names the place and lists mapped water, parks/reserves, natural cover, land use, roads and paths, and an approximate building count around the flight, using OpenStreetMap. Edit it as needed, since OpenStreetMap can be incomplete. This needs an internet connection; offline you get the coordinates and flight extent only.

**3. Work on each observation** (one row per frame):
- **Frame:** ▲▼ reorder rows; **« 2s ‹ 1f 1f › 2s »** step to a nearby frame if you paused slightly off (notes are kept; drawings are cleared); **▶ video** jumps the player to that moment.
- **Location data:** clock time, video time, latitude/longitude, altitude, camera pitch (degrees from nadir, with a plain-language description), camera yaw with compass direction, the **view** (ground footprint width × depth and cm per pixel), plus a side-view pitch diagram and a top-view yaw diagram. "(assumed)" means the SRT has no gimbal pitch and the Pitch setting under the video is being used.
- **Camera tilt and yaw:** many DJI SRT files (Mavic 3, Air 3, Mini 4 and others) don't record the gimbal angle, so the viewer starts by assuming the camera points straight down. The PITCH line then says *(assumed — set tilt below)* and an amber banner appears above the rows. For oblique shots, fix it in the **Camera** box under the diagrams:
  - **Tilt** — type degrees from straight down (0 = nadir, 90 = looking at the horizon), or
  - **⌖ Click horizon** — then click on the horizon line in the image and the tilt is calculated for you (uses the HFoV setting, so pick your **Drone / camera** at the top of the report first).
  - **Yaw** — type the direction the camera faced if the SRT heading is wrong or comes from the GPS track.
  - **Tilt → all frames** copies the tilt to every frame without SRT pitch, and new captures inherit the last tilt you set. **↺ Auto** returns to the SRT / default values.
  - The pitch diagram, footprint, map cones and any measurements update immediately.
- **Notes:** type observations for that frame.
- **↔ Measure distance:** click two points on the image to get an approximate ground distance (labelled M1, M2…). It uses the same camera model as the footprint, so set the HFoV/Pitch first. It assumes flat ground at takeoff height, so treat it as an estimate, especially for oblique views, slopes and tall objects.
- **Draw:** arrow, box, ellipse, freehand and text label in five colours, with ↶ undo and ✕ clear. `Esc` stops the current tool.
- **🗑 Delete row** removes the observation.

**4. Print / Save PDF.** Click **🖨 Print / Save PDF** and choose **Save as PDF** as the printer. The printed report uses a clean light layout: title block, flight summary and map, area description, then one observation per page with the annotated image, location data, diagrams, notes and measurements, plus page numbers.

> **Your work is autosaved in this browser.** If you close or refresh the page, click **Restore** in the Captures tray (or in **📝 Report**). The save lives only in this browser on this computer (not in your files), so print to PDF to keep a permanent copy. Private/incognito windows may not keep it.

---

## Frame Grabs

📸 captures frames into the report. To save image files instead, click **▾** next to 📸. Each option downloads the frame you're looking at as a JPG at full video resolution to your browser's Downloads folder. Tick **Also download a geotagged JPG with every 📸 capture** to get both at once.

- **Download frame** — a clean, geotagged JPG. The drone's position (interpolated between SRT records), altitude, camera direction and the SRT timestamp are written into the photo's EXIF GPS tags, so it drops straight onto a map in QGIS, ArcGIS Pro, Google Earth or Lightroom. Gimbal yaw/pitch/roll go into DJI-style XMP tags that WebODM, Metashape and Pix4D read.
- **Download frame + telemetry overlay** — the same image with a caption bar (file, time, coordinates, altitude, speed, heading, gimbal pitch), a north arrow when the camera direction is known, and a rough scale bar when the SRT says the camera was pointing straight down. Good for reports and slides.
- **Extract frames every N seconds…** — steps through the video and saves a ZIP containing the geotagged JPGs, `frames.csv` (all telemetry for each image), `geo.txt` (OpenDroneMap/WebODM geolocation file) and a short README. Set the interval and an optional From/To range (m:ss or seconds); the dialog estimates the frame count and ZIP size before you start, and **Stop** saves what's done so far.

> **Photogrammetry from video:** For ~75% forward overlap, use an interval of about 0.25 × footprint depth ÷ speed (the footprint readout gives the depth). Video frames have motion blur, rolling shutter and compression, so expect a rougher model than from mapping stills — which is a useful lesson in itself. Leave the overlay off for frames you'll process.

> If no SRT is loaded, frames are still saved but without GPS tags.

---

## Camera Footprint

Click **📐 Footprint** in the map toolbar to project the four corners of the video frame onto the ground and draw them on the map as the video plays. The HFoV, Pitch and Hdg± calibration controls appear under the video while the footprint is on, and a panel above the map offers:

- **View cone** — dashed rays from the drone to each corner of the image.
- **Coverage trail** — leaves a footprint on the map every N seconds as the video plays, showing which ground the camera actually saw and how much consecutive views overlap. **▦ Whole flight** draws the trail for the entire flight at once; **✕ Clear** removes it.
- **Max range** — for oblique views, the top of the frame can point at or above the horizon. Instead of hiding the footprint, the far edge is cut at this distance and drawn dashed.
- **Readout** — footprint width × depth on the ground, the video's ground sample distance (cm per pixel at the frame centre) and the slant range to the centre of the view.

The footprint uses live gimbal pitch/yaw from the SRT when the file has them; otherwise it uses the Pitch override (default −90°, straight down) and the drone's heading. It assumes flat ground at takeoff height, so hills, buildings and tree canopy shift the real footprint. Pick your drone in the **Model…** list to set the field of view.

---

## 3D Flight View & Altitude Profile

Click **🧊 3D** (or press `3`) to replace the map with a 3D view of the flight. Click **🗺 Map** to go back.

- The track is colored by altitude (same colors as the map) with a gray shadow on the ground and vertical "curtain" lines, so you can read height at a glance. The green **H** is the home point; *start* and *end* mark the ends of the recording.
- The blue dot is the drone's current position and moves with the video; when the footprint is on, the camera's view cone and ground footprint are drawn in 3D too.
- **Mouse:** drag to rotate, right-drag or Shift-drag to pan, scroll to zoom, double-click to reset. Click anywhere on the track to jump the video to that moment. On touch screens, drag with one finger to rotate and pinch to zoom.
- **Vertical** — exaggerate altitude (×1 to ×10) to make small height changes visible on long, low flights.
- **Follow** keeps the drone centered while the video plays. **Top** and **Side** jump to plan and profile views.

Under the 3D view, the **altitude profile** plots altitude against distance flown along the track (rather than time, as in 📊 Charts). Hover for distance, altitude and time; click to jump the video there.

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
- Turn on the coverage trail and 3D view together to show how altitude and gimbal pitch change what the camera sees — then try **Extract frames** and run them through WebODM to compare a video-derived model with one from a planned mapping mission.
- The inspection report turns a flight video into a structured field-observation assignment: students pick frames, annotate and measure features, describe the site, and submit the PDF to Canvas.
- Geotagged frame grabs give students georeferenced evidence for field observations (erosion, damage, vegetation) without a separate photo flight.
