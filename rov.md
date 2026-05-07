ROV Dashboard PWA — Complete Master Prompt Guide

What You're Building
A PWA (Progressive Web App) ROV Dashboard that runs on your phone with:

📷 Rear camera + live object detection
🗺️ GPS live map
⚠️ Crack / Algae / Sludge / Corrosion detection
📱 Installable on home screen, works offline


File Structure (Final Goal)
rov-dashboard/
├── index.html
├── manifest.json
├── sw.js
└── icons/
    ├── icon-192.png
    └── icon-512.png

Your Side Requirements
Before starting, make sure you have:
ToolPurposeDownloadVS CodeCode editorcode.visualstudio.comLive Server extensionServe files locallyInstall inside VS CodeChrome on phonePWA supportAlready installedNetlify accountFree hostingnetlify.comSame WiFi/HotspotTesting locallyYour phone + laptop on same network

Phase Breakdown
PhaseWhat Gets BuiltPrompt NumberPhase 1Project skeleton + PWA shellPrompt 1Phase 2Dashboard UI layoutPrompt 2Phase 3Webcam / rear camera feedPrompt 3Phase 4TensorFlow object detectionPrompt 4Phase 5Crack/Algae/Sludge heuristicsPrompt 5Phase 6GPS + Leaflet mapPrompt 6Phase 7Detection log + alert systemPrompt 7Phase 8PWA manifest + service workerPrompt 8Phase 9Deploy to NetlifyGuide (no prompt)


PHASE 1 — Project Skeleton + PWA Shell
What This Does
Creates the 3 core files with correct structure so your browser recognises this as a PWA from day one. Nothing visible yet — just the foundation.
Your Side (Before Prompting)

Create a folder on your desktop called rov-dashboard
Open VS Code → File → Open Folder → select rov-dashboard
Create 3 empty files: index.html, manifest.json, sw.js
Create a folder inside called icons

Prompt 1 — Copy This Exactly
I am building a PWA ROV Dashboard app. 
This is Phase 1 — Project Skeleton only.

Create the following 3 files:

FILE 1: index.html
- Basic HTML5 boilerplate
- Title: "ROV Dashboard"
- Link to manifest.json in the <head>
- Add a <meta name="theme-color" content="#0a0f1e">
- Add a <meta name="viewport" content="width=device-width, initial-scale=1.0">
- Register sw.js service worker inside a <script> tag at bottom
- Body should just have one <div id="app"> with text "ROV Dashboard Loading..." for now
- No CSS yet, no libraries yet

FILE 2: manifest.json
- name: "ROV Dashboard"
- short_name: "ROV"
- start_url: "/"
- display: "fullscreen"
- background_color: "#0a0f1e"
- theme_color: "#00e5ff"
- orientation: "portrait"
- icons array with icon-192.png and icon-512.png paths pointing to /icons/ folder

FILE 3: sw.js
- Basic service worker
- On install: cache these files: '/', '/index.html', '/manifest.json'
- On fetch: try cache first, fallback to network
- Cache name: "rov-cache-v1"

Give me all 3 files completely. No explanations, just the code.
After Phase 1 — What You Do

Paste each file's code into the correct file in VS Code
Add any 2 PNG images named icon-192.png and icon-512.png into the icons folder (just use any image for now, we'll replace later)
In VS Code, right click index.html → Open with Live Server
Browser opens at http://localhost:5500 — you should see "ROV Dashboard Loading..."
✅ Phase 1 done



PHASE 2 — Dashboard UI Layout
What This Does
Builds the full visual layout — dark theme, panels, navbar, stats bar. No live data yet, everything is placeholder/static. This is purely HTML + CSS.
Your Side (Before Prompting)

Phase 1 must be working and visible in browser
Have index.html open in VS Code

Prompt 2 — Copy This Exactly
I am building a PWA ROV Dashboard. 
This is Phase 2 — UI Layout only. No JavaScript logic yet.

Update index.html with the following layout and styling. 
Keep the PWA-related tags already in the <head> and the service worker 
registration script already at the bottom. Only add to them, never remove.

LAYOUT STRUCTURE (CSS Grid):
The page has 4 sections:

1. TOP NAVBAR (height: 60px)
   - Left: ROV icon (use 🤖 emoji) + "ROV Dashboard" text
   - Center: Live clock placeholder (static text "00:00:00" for now, id="clock")
   - Right: Status badge saying "● SYSTEM ONLINE" in green

2. MAIN CONTENT AREA (below navbar, above stats bar, fills remaining height)
   Split into two columns:
   - LEFT COLUMN (60% width): 
       One panel taking full height labeled "CAMERA FEED"
       Inside: a <canvas id="feed-canvas"> placeholder with text 
       "Camera initializing..." centered in it
   - RIGHT COLUMN (40% width):
       Top half: panel labeled "GPS LOCATION"
         Inside: <div id="map"> with text "Map loading..." 
         Below map div: two small text lines: 
         "LAT: --" (id="lat-display") and "LNG: --" (id="lng-display")
       Bottom half: panel labeled "DETECTION LOG"
         Inside: scrollable <div id="log-container"> 
         with 3 fake placeholder log entries for visual preview
         Add a "CLEAR LOG" button at top right of this panel

3. BOTTOM STATS BAR (height: 55px)
   5 metric cards side by side:
   - 🔋 Battery: 87%
   - 📡 Signal: Strong  
   - 🌊 Depth: 4.2m
   - ⚡ Speed: 0.8 m/s
   - 🎯 Mode: Autonomous

4. ALERT BANNER (hidden by default, id="alert-banner")
   Fixed position at top of screen
   Red background, white text, centered
   Text: "⚠️ STRUCTURAL ANOMALY DETECTED"
   display:none by default

DESIGN RULES:
- Background: #0a0f1e (dark navy)
- Accent: #00e5ff (cyan)
- Alert red: #ff3d57
- Panels: #0d1526 background, 1px solid #1a2a4a border, border-radius 8px
- All text: white
- Font: use Google Font "Rajdhani" for headers, "DM Sans" for body text
- Panel labels: uppercase, cyan color, letter-spacing 2px, font-size 11px
- Canvas placeholder: #0a1929 background, dashed cyan border
- Log entries: small text, monospace, each with a left colored border
- Stats bar: flex row, each card has cyan top border, dark background

Make it look like a professional military/industrial control dashboard.
Mobile-first: everything should fit on a phone screen (390px width) without horizontal scroll.
After Phase 2 — What You Do

Paste the updated index.html code
Save → Live Server auto-refreshes
You should see the full dark dashboard layout with all panels visible
Check on your phone too — open http://YOUR-LAPTOP-IP:5500 on phone browser
✅ Phase 2 done if layout looks correct on both



PHASE 3 — Rear Camera Feed
What This Does
Activates your phone's rear camera and streams it live onto the canvas. No detection yet — just raw camera feed showing on screen.
Your Side (Before Prompting)

Phase 2 layout must be visible
Test on phone (not laptop) from Phase 3 onwards since rear camera only exists on phone

Prompt 3 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 3 — Rear Camera Feed only.

The index.html already has:
- A <canvas id="feed-canvas"> inside the camera panel
- PWA tags and service worker registration

Add JavaScript (inside a <script> tag before </body>, 
after the existing service worker script) that does:

1. CAMERA SETUP:
   - Request rear camera using: 
     { video: { facingMode: { exact: "environment" }, 
       width: { ideal: 1280 }, height: { ideal: 720 } } }
   - If rear camera fails (exact constraint fails on some phones), 
     fallback to: { video: { facingMode: "environment" } }
   - If that also fails, fallback to: { video: true } (any camera)
   - Show user-friendly error if all fail: update canvas to show 
     "❌ Camera Access Denied — Please allow camera permission"

2. CANVAS RENDERING:
   - Create a hidden <video> element (not added to DOM)
   - Set video.srcObject to the camera stream
   - On video loadedmetadata: resize canvas to match video dimensions
   - Use requestAnimationFrame loop to draw video frames onto canvas
     using ctx.drawImage(video, 0, 0, canvas.width, canvas.height)

3. LIVE CLOCK:
   - Update the element with id="clock" every second
   - Format: HH:MM:SS using toLocaleTimeString()

4. CLEAR LOG button:
   - Wire up the CLEAR LOG button to clear all children of #log-container

5. SNAPSHOT button (add a small button overlaid on camera panel):
   - Position: absolute, bottom-right of camera panel
   - Text: "📷 SNAP"
   - On click: canvas.toBlob() → create download link → auto click it
   - Filename: "rov-snap-[timestamp].png"

Do not touch the HTML structure or CSS. Only add JavaScript.
No TensorFlow yet. Just camera feed + clock + snapshot.
After Phase 3 — What You Do

Paste the JS code into index.html
Open on your phone browser at http://YOUR-LAPTOP-IP:5500
Phone will ask camera permission → tap Allow
You should see live rear camera feed on the canvas
Test the SNAP button — it should download a photo
✅ Phase 3 done if camera shows on phone



PHASE 4 — TensorFlow Object Detection
What This Does
Loads TensorFlow.js and COCO-SSD model. Detects everyday objects (person, bottle, phone etc.) with cyan bounding boxes and labels drawn on the canvas over the camera feed.
⚠️ Warning
This phase loads a ~5MB ML model. On first load it will take 10–20 seconds. This is normal. After that it's cached.
Your Side (Before Prompting)

Phase 3 camera must be working on phone
Make sure phone has internet connection for model download

Prompt 4 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 4 — TensorFlow COCO-SSD Object Detection.

The index.html already has:
- Working rear camera feed drawing to canvas id="feed-canvas"
- requestAnimationFrame loop already running
- A <script> block with camera code

Add the following. Do not break existing camera code:

1. ADD TO <head> (CDN scripts, load before existing scripts):
   <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.10.0/dist/tf.min.js"></script>
   <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/coco-ssd@2.2.2/dist/coco-ssd.min.js"></script>

2. LOADING SCREEN:
   Add a <div id="loading-screen"> overlaid fullscreen on top of everything
   Dark background, centered text:
   "🤖 Loading ROV Systems..."
   Below it: a thin cyan animated progress bar (CSS animation, fake fill over 8 seconds)
   Hide this div once model is loaded (set display:none)

3. MODEL LOADING:
   After camera starts, load COCO-SSD:
   const model = await cocoSsd.load()
   Hide loading screen after model loads

4. DETECTION LOOP:
   Run model.detect(canvas) every 300ms using setInterval
   Store results in a variable called cocoDetections

5. DRAWING DETECTIONS:
   Modify the existing requestAnimationFrame loop:
   After ctx.drawImage(video...) add a function drawDetections() that:
   - Loops through cocoDetections array
   - For each detection with score > 0.55:
     - Draw rectangle: 2px cyan (#00e5ff) stroke
     - Semi-transparent fill: rgba(0, 229, 255, 0.08)
     - Label badge: filled cyan rectangle at top-left of box
     - Label text: white, bold, 13px, format: "OBJECT NAME 94%"
   - Skip drawing if cocoDetections is empty or null

6. ADD TO LOG:
   Create a function addLog(label, color, confidence):
   - Creates a new <div> in #log-container
   - Format: "[HH:MM:SS] ● LABEL — XX%"
   - Left border 3px solid color
   - Padding 6px
   - Font: monospace, 11px
   - Auto scroll log to bottom after adding
   - Call this function whenever a new unique object is detected
     (only log if that label wasn't logged in the last 5 seconds,
     use a Map to track last log time per label)

Do not rewrite existing code. Only add to it.
Keep camera feed working. Keep snapshot working. Keep clock working.
After Phase 4 — What You Do

Add CDN scripts to <head> of index.html
Add the loading screen HTML + CSS
Add detection JS code
Open on phone → wait for "Loading ROV Systems..." to finish
Point camera at objects — you should see cyan boxes with labels
Check detection log panel is getting entries
✅ Phase 4 done if objects get detected and labeled



PHASE 5 — Crack / Algae / Sludge / Corrosion Detection
What This Does
Adds pixel-level heuristic scanning on top of TensorFlow. Analyzes camera frame colors to detect tunnel defects and draws colored boxes with labels.
Your Side (Before Prompting)

Phase 4 must be working with COCO-SSD detection visible

Prompt 5 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 5 — Underwater Tunnel Defect Detection (Heuristic Layer).

The index.html already has TensorFlow COCO-SSD running.
The canvas is id="feed-canvas", 2D context is ctx.

Add a new JavaScript function called scanForDefects() that:

SCANNING METHOD:
- Run every 600ms using setInterval
- Get pixel data: ctx.getImageData(0, 0, canvas.width, canvas.height)
- Divide canvas into a grid of 24x24 pixel blocks
- For each block: sample the center pixel's R, G, B values
- Classify each block into one of 4 defect types or "none"

CLASSIFICATION RULES (per block center pixel):

CRACK:
- Brightness = (R+G+B)/3 < 35
- Aspect: found in clusters of 3+ adjacent dark blocks in a row (horizontal or diagonal)
- Box color: #ff3d57 (red)
- Label: "⚠ CRACK"
- Log color: #ff3d57

ALGAE:
- G > 90 AND G > R + 25 AND G > B + 15
- Cluster of 4+ adjacent green blocks
- Box color: #39ff14 (bright green)
- Label: "🟢 ALGAE"
- Log color: #39ff14

SLUDGE:
- R between 35-110, G between 25-80, B between 15-65
- Overall brightness (R+G+B)/3 between 25-80
- Cluster of 5+ adjacent blocks
- Box color: #ff8c00 (orange)
- Label: "🟠 SLUDGE"  
- Log color: #ff8c00

CORROSION:
- R > 130 AND G < 85 AND B < 65
- Cluster of 3+ adjacent blocks
- Box color: #ffd700 (gold)
- Label: "🔶 CORROSION"
- Log color: #ffd700

DRAWING DETECTED DEFECT BOXES:
- After classifying all blocks, group adjacent same-type blocks into regions
- Draw one bounding box per region (not per block)
- Box: 2px stroke in defect color
- Fill: rgba version of color at 0.1 opacity
- Label badge: same style as COCO-SSD labels but in defect color
- Draw these on top of the video frame in the animation loop
  (store defect regions in a variable, draw in drawDetections())

ALERT SYSTEM:
- If CRACK or CORROSION region is detected:
  - Show #alert-banner (set display:block)
  - Flash navbar background to #ff3d57 for 1 second then back to original
  - Play a beep: Web Audio API, oscillator, 880Hz, 0.3 seconds, sine wave
  - Auto-hide alert banner after 4 seconds
  - Do not repeat alert more than once every 8 seconds (use a timestamp flag)

- Call addLog() for each defect type detected
  (same 5-second cooldown per label as COCO-SSD logs)

Do not modify TensorFlow code. Do not modify camera code.
Only add scanForDefects() and wire it into the animation loop.
After Phase 5 — What You Do

Add the defect scanning code
Test by pointing camera at:

Dark surface = should trigger CRACK
Green plant/leaf = should trigger ALGAE
Brown muddy surface = should trigger SLUDGE
Rusty/orange surface = should trigger CORROSION


Check alert banner appears for cracks
Check beep sound plays
✅ Phase 5 done if defect boxes appear



PHASE 6 — GPS + Leaflet Map
What This Does
Gets live GPS from phone and shows a moving marker on a real map using Leaflet.js + OpenStreetMap tiles. Updates every 3 seconds. Shows lat/lng below map.
Your Side (Before Prompting)

Phase 5 must be working
Test this phase on phone only (laptop has no GPS)

Prompt 6 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 6 — GPS and Leaflet Map.

The index.html already has:
- A <div id="map"> inside the GPS panel
- Elements id="lat-display" and id="lng-display"
- All previous camera and detection code

ADD TO <head>:
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

ADD JAVASCRIPT for GPS + Map:

1. MAP INITIALIZATION:
   - Initialize Leaflet map inside div id="map"
   - Default center: Chennai [13.0827, 80.2707], zoom 15
   - Tiles: OpenStreetMap standard tiles
     "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
   - Attribution: "© OpenStreetMap"
   - Disable zoom controls (zoomControl: false) to save space
   - Create a marker at default center
   - Marker should be a custom cyan pulsing dot:
     Use L.divIcon with a CSS class "rov-marker"
     Style: 14px circle, cyan background, box-shadow pulse animation

2. GPS TRACKING:
   - Call navigator.geolocation.watchPosition() with options:
     enableHighAccuracy: true, maximumAge: 3000, timeout: 10000
   - On success:
     - Move marker to new [lat, lng]
     - Pan map to new position (map.panTo)
     - Update #lat-display text: "LAT: 13.082341"
     - Update #lng-display text: "LNG: 80.270712"
     - Show up to 6 decimal places
   - On error:
     - Update lat/lng display: "LAT: Unavailable"
     - Show small text below map: "⚠ GPS signal weak"
     - Keep map centered on Chennai default

3. MAP SIZING FIX:
   After map initializes, call map.invalidateSize() after 500ms delay
   (fixes Leaflet rendering inside flex/grid containers)

Do not touch any camera, TensorFlow, or defect detection code.
Only add map and GPS code.
After Phase 6 — What You Do

Add Leaflet CSS/JS to head
Add GPS + map JS code
Open on phone (GPS needs phone)
Allow location permission when asked
You should see your real location on the map with a marker
Walk around — marker should move
✅ Phase 6 done if map shows and marker moves



PHASE 7 — Detection Log + Alert Polish
What This Does
Polishes the detection log, adds timestamps, color coding, auto-scroll, and makes the alert system smooth. Also adds the live stats bar animation.
Your Side (Before Prompting)

Phase 6 must be working

Prompt 7 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 7 — Detection Log Polish + Alert System Finalization.

The index.html already has all camera, detection, GPS working.
The addLog() function exists. The #log-container exists. #alert-banner exists.

Make the following improvements WITHOUT breaking existing code:

1. LOG IMPROVEMENTS:
   - Each log entry: pill-shaped label badge + timestamp + message
   - Format: [badge with colored bg] [HH:MM:SS] Description — XX%
   - Alternating row backgrounds: #0d1526 and #111d30
   - Max 50 entries in log — if more, remove oldest
   - Smooth fade-in animation on new entries (CSS: opacity 0 to 1, 0.3s)
   - CLEAR LOG button: confirm before clearing 
     ("Clear all detection logs?" confirm dialog)

2. ALERT BANNER IMPROVEMENTS:
   - Slide down animation when appearing (CSS transform translateY)
   - Shows the specific defect type: "⚠️ CRACK DETECTED — Structural Risk"
   - Include timestamp in alert
   - Dismiss button (✕) on right side of banner
   - Auto dismiss after 4 seconds with a countdown shown: "Auto-dismiss in 3..."

3. STATS BAR — MAKE BATTERY ANIMATE:
   - Battery stat: randomly fluctuate between 85-89% every 10 seconds
     (simulates live data)
   - Signal stat: if GPS is active show "● Strong" in green, 
     if GPS error show "◌ Weak" in orange
   - Mode stat: cycle between "Autonomous" and "Scanning" every 8 seconds

4. ADD A SMALL INDICATOR on camera panel:
   - Top-left overlay badge: "● LIVE" blinking red dot + "REC" text
   - Top-right: detection count badge showing how many objects currently detected
     format: "3 OBJECTS" — updates every second

5. SYSTEM STATUS in navbar:
   - If camera active + GPS active: "● ALL SYSTEMS ONLINE" green
   - If camera active but no GPS: "◐ CAM ONLY" yellow  
   - If camera failed: "○ OFFLINE" red
   - Update this every 5 seconds

Do not rewrite any detection or camera code.
Only add polish, animations, and UI improvements.
After Phase 7 — What You Do

Add the polish code
Test all log entries appear with colors
Test alert banner slides in and dismisses
Check LIVE indicator blinks on camera panel
✅ Phase 7 done if dashboard looks complete and polished



PHASE 8 — PWA Manifest + Service Worker Finalization
What This Does
Finalizes PWA config so you can install the dashboard as an app on your phone home screen. Works offline after first load.
Your Side (Before Prompting)

All phases 1-7 working
You need to create icon images (instructions below)

Creating Icons (Your Job)

Go to https://favicon.io or https://canva.com
Create a simple icon — dark background, 🤖 or ROV text in cyan
Download as PNG
Resize to 192x192 → save as icons/icon-192.png
Resize to 512x512 → save as icons/icon-512.png

Prompt 8 — Copy This Exactly
I am building a PWA ROV Dashboard.
This is Phase 8 — Final PWA Configuration.

Give me the final versions of these 2 files:

FILE 1: manifest.json
{
  Complete and correct manifest with:
  - name: "ROV Dashboard"
  - short_name: "ROV"
  - description: "Autonomous ROV monitoring dashboard"
  - start_url: "/index.html"
  - display: "fullscreen"
  - orientation: "portrait-primary"
  - background_color: "#0a0f1e"
  - theme_color: "#00e5ff"
  - icons: icon-192.png (purpose: any) and icon-512.png (purpose: any maskable)
  - categories: ["utilities", "productivity"]
  - screenshots array with one entry (just a placeholder structure)
}

FILE 2: sw.js
Complete service worker with:
- Cache name: "rov-v2"
- Files to cache on install:
  '/', '/index.html', '/manifest.json',
  '/icons/icon-192.png', '/icons/icon-512.png',
  The TensorFlow CDN URLs,
  The Leaflet CDN URLs
- Fetch strategy: 
  For same-origin requests: cache first, network fallback
  For CDN requests (jsdelivr, unpkg, openstreetmap): 
  network first, cache fallback
- Activate: delete old caches (any cache not named "rov-v2")
- Skip waiting and claim clients immediately

Give me both files completely. No explanations.
After Phase 8 — What You Do

Replace manifest.json and sw.js with new versions
Hard refresh browser (Ctrl+Shift+R)
Open on phone → Chrome shows "Add to Home Screen" banner at bottom
Tap it → give app a name → tap Add
Go to home screen → tap the ROV Dashboard icon
Opens fullscreen like a real app ✅



PHASE 9 — Deploy to Netlify (No Prompt Needed)
What This Does
Puts your app on the internet so you can open it anywhere, anytime, on any phone — without needing your laptop running.
Step by Step
Step 1 — Go to netlify.com
Create a free account (use Google login)
Step 2 — Deploy

Go to dashboard → click "Add new site" → "Deploy manually"
Drag and drop your entire rov-dashboard folder onto the page
Netlify uploads everything and gives you a URL like:
https://random-name-123.netlify.app

Step 3 — Open on Phone

Open that URL on your phone's Chrome
Allow camera + location permissions
Chrome shows "Add to Home Screen" → tap it
App installs on home screen ✅

Step 4 — Update Later
If you change code, just drag-drop the folder again on Netlify → it updates automatically. Your installed app refreshes next time you open it.


Full Summary — What Does What
ComponentTechnologyWhere It RunsCamera feedgetUserMedia APIPhone browserObject detectionTensorFlow COCO-SSDPhone browser (ML on device)Defect detectionCanvas pixel analysisPhone browserGPS trackingnavigator.geolocationPhone GPS chipLive mapLeaflet.js + OpenStreetMapPhone browserOffline supportService Worker cachePhone browserApp installPWA manifestChrome on phoneHostingNetlify (free)Internet

Important Notes

Always test on phone from Phase 3 onwards — rear camera and GPS don't work on laptop
First load needs internet — model download is ~5MB
After first load, works offline — service worker caches everything
Each prompt builds on the previous — never skip a phase
If something breaks — tell your AI tool exactly which phase broke and paste the error message
