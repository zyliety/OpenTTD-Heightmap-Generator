# OpenTTD-Heightmap-Generator

![2026-08-06 115833.png](https://files.seeusercontent.com/2026/08/06/lg7X/2026-08-06-115833.png)

🌟 Project Overview
OpenTTD Heightmap Generator is a modern, high-precision web tool designed to generate realistic 8-bit heightmaps and extract real-world town data for OpenTTD.Powered by OpenStreetMap (OSM) vector data and Terrarium DEM global elevation tiles, it enables players to select any geographic region on Earth, fine-tune terrain smoothing and water levels, and export game-ready PNG heightmaps along with structured JSON town configurations in one click.

### Try Out: https://zyliety.github.io/OpenTTD-Heightmap-Generator.html

### Note:

* Do not select an area that is too large. It is recommended to keep both the length and width under 8192 pixels, otherwise the export speed will be significantly affected.

* Do not set the Town Filter Threshold parameter too high, as it may cause town data fetching to fail.

### ✨ Key Features & Highlights

#### 1. 🍎 Friendly UI & Dual Theme System

* **Day / Night Mode Switch**: Starts in Light Mode by default. Effortlessly switch to Dark Mode via the top-right toolbar button.
* **Fixed Top-Right Control Bar**: Theme toggle and language selection are fixed at the top-right corner to ensure uncluttered map interactions.

#### 2. 📐 Fixed Aspect-Ratio Region Selection

* **Standard OpenTTD Tile Presets**: Supports official tile resolutions ranging from `64x64` up to `1048576`.
* **Locked Aspect Ratio Box**: The selection box automatically locks its proportions to match the selected map dimensions, preventing geographic distortion.
* **Shortcut Drag-to-Move (`Ctrl + Drag`)**: Hold the `Ctrl` key and drag the box to reposition the selection area seamlessly across the globe.

#### 3. 🏔️ DEM Elevation Processing & Terrain Smoothing

* **Bilinear Interpolation Sampling**: Smoothly resamples elevation tiles to prevent pixelation artifacts in scaled maps.
* **Precision Sea Level & Altitude Mapping**: Customize sea level cutoffs and maximum mountain elevations (supporting OpenTTD's maximum 256 height levels).
* **Multi-pass Box Filter Smoothing**: Flatten jagged mountain cliffs into gentler slopes that are ideal for building railway networks and roads.
* **Inland Lake Detection & Infill**: Built-in flood-fill algorithm automatically detects inland basins below sea level and fills them to prevent unnatural dry craters.
* **Custom Low-Elevation Offset**: Option to forcibly raise coastal or sub-sea-level land to specified target elevations.

#### 4. 🏙️ Real-World OSM Town Data Extraction

* **Live Overpass API Queries**: Fetches cities, towns, villages, and suburbs with their real-world names, coordinates, and populations within the box.
* **Multi-Mirror Endpoint Failover**: Automatically cascades across multiple Overpass API mirrors (`Overpass-API`, `Kumi`, `Mail.ru`) to ensure maximum reliability.
* **Population Scaling & Filter Thresholds**: Adjust population scale factors and filter towns based on OSM place ranks to control map node density.
* **Smart `city` Flag Assignment**: Automatically tags major urban areas with `city: true` based on place hierarchy.

#### 5. 📦 Multiple Export Formats & One-Click ZIP Packaging

* **Real-time Canvas Preview**: View the grayscale heightmap on an integrated preview canvas before downloading.
* **Multiple Town JSON Formats**:
* **OpenTTD / RoadTycoon Official Spec**: Normalized ratio coordinates (0.0–1.0) with swapped X/Y axes and `city` boolean flag.
* **Standard GameScript (GS)**: Absolute tile integer coordinates (`x`, `y`) with scaled population.
* **Minimal Ratio Mode**: Clean ratio coordinates with town names only.


* **One-Click ZIP Packaging**: Powered by JSZip, export both the PNG heightmap and Towns JSON together into a single downloadable ZIP file.

#### 6. 🌐 Intuitive Interaction & Multi-language Support (i18n & Tooltips)

* **Native i18n**: Seamless switching between **English (EN)**, **Chinese (ZH)**, and **Japanese (JA)**.
* **Hover Parameter Documentation**: Hovering over any parameter section displays an interactive tooltip explaining its usage based on OpenTTD & RoadTycoon specifications.
* **One-Click Reset Button**: Easily revert all sliders, checkboxes, and selections back to default baseline values.

---

### 🛠️ Tech Stack

* **Frontend**: HTML5, CSS3 (Custom Variables, Backdrop Blur Filters), Pure JavaScript (ES6+)
* **Map Engine**: [Leaflet.js](https://www.google.com/search?q=https://leafletjs.com/) (CartoDB Dark Tiles & OpenStreetMap Light Tiles)
* **DEM Data Source**: Amazon S3 Terrarium Elevation Tiles
* **GIS API**: OpenStreetMap Overpass API
* **Archiving**: [JSZip](https://www.google.com/search?q=https://stuk.github.io/jszip/)
* **Bunburya's script**: Bunburya's OpenTTD Heightmap Generator

### 🔰 Parameter Beginner's Guide

#### 1. Water Level Cutoff

* **Function**: Determines the elevation altitude below which pixels are recognized as sea/water in the heightmap.
* **Recommended Setting**: `0m` (default), fine-tune based on shorelines.
* **How to Use**:
* **Land flooded**: Move slider left to lower the cutoff (e.g., `-5m` to `-10m`).
* **Sea turning into land**: Move slider right to raise the cutoff (e.g., `5m` to `10m`).



#### 2. Max Elevation

* **Function**: Maps the highest real-world mountain peak in your selection to OpenTTD's maximum height steps (up to 256 height levels).
* **Recommended Setting**: `64 - 128` for flat/manageable maps, `192 - 256` for extreme mountainous terrain.
* **How to Use**:
* **For easy track laying**: Set lower values (`64` or `96`) to keep slopes gentle.
* **For mountainous challenges**: Set higher values (`256`) to create steep cliffs and challenging alpine passes.



#### 3. Terrain Smoothing (Box Filtering)

* **Function**: Applies a smoothing algorithm to raw elevation tiles to soften jagged noise and sheer cliffs.
* **Recommended Setting**: `Low (Level 1)` or `Medium (Level 2)`.
* **How to Use**:
* **Off**: Retains 100% raw topographic detail, but produces numerous 1x1 micro-cliffs.
* **Low / Medium**: Converts sharp mountain steps into smooth slope gradients. Highly recommended for beginners.



#### 4. Force Lift Sea Level & Below Pixels

* **Function**: Forcibly elevates land areas that are at or below sea level by a specified offset, preventing them from being submerged.
* **Recommended Setting**: **Checked** when mapping low-lying coastal regions (e.g., Netherlands, Venice).
* **How to Use**: Enable this to lift sub-sea-level land `1–2` tiles above water so towns and roads can spawn normally.

#### 5. Force Lift Low-Elevation Land

* **Function**: Ensures all land tiles near the coastline maintain a distinct minimum height step above water to avoid rendering artifacts.
* **Recommended Setting**: **Checked** (Offset `1` tile) for maps with complex coastlines.
* **How to Use**: Gives coastal plains a clean, raised embankment above the sea, simplifying seaport and railway construction.

#### 6. Inland Lake Detection & Infill

* **Function**: Uses a flood-fill algorithm to separate ocean coastal water from inland below-sea-level basins, automatically filling dry interior depressions.
* **Recommended Setting**: **Checked** for inland mountain maps; **Unchecked** if you want real-world lakes (e.g., Great Lakes) to generate as water.
* **How to Use**:
* **Checked**: Prevents deep inland valleys from accidentally turning into giant dry ocean craters.
* **Unchecked**: Keeps real inland lakes flooded as playable water bodies.



#### 7. Population Scale

* **Function**: Multiplies real-world OpenStreetMap population figures by the specified scale factor to convert them into balanced starting populations for OpenTTD.
* **Calculation Formula**:

$$\text{In-Game Population} = \text{Real OSM Population} \times \text{Population Scale}$$


* **Examples**:
* **Example 1 (Scale = `0.01` / 1%)**: Real metropolis with **1,000,000** residents becomes $1,000,000 \times 0.01 = 10,000$ in-game population; town of **50,000** becomes $500$. *(Recommended baseline)*
* **Example 2 (Scale = `0.05` / 5%)**: A city with **1,000,000** residents yields $50,000$ in-game population. *(Ideal for high-density passenger network challenges)*
* **Example 3 (Scale = `0.001` / 0.1%)**: A city of **1,000,000** becomes $1,000$ residents; town of **50,000** shrinks to $50$. *(Great for lightweight freight-focused maps)*



#### 8. Town Filtering Threshold

* **Function**: Filters out small settlements or low-priority nodes based on population size or administrative rank to prevent map clutter.
* **Threshold Values (`0`, `0.5`, `1`)**:
* **Set to `0` (No Filter / Keep All)**: Retains 100% of OSM nodes in the selection (including small villages, tiny hamlets, and farmsteads). *(Best for micro-regional or rural maps)*
* **Set to `0.5` (Medium Threshold / Balanced Clean-up)**: Automatically purges minor hamlets, keeping medium-to-large towns and main cities intact. *(Best for standard maps)*
* **Set to `1` (High Threshold / Major Cities Only)**: Strips away almost all minor towns to keep only top-tier urban centers. *(Best for large High-Speed Rail or Airline maps)*
