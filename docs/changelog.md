# Changelog

## v1.0 — 2026-02-23

### Initial build
- Created interactive UK map plotting 130 accounts by postcode with revenue-sized bubbles
- Geocoded 129 unique postcodes via postcodes.io bulk API (122 matched first pass)
- Added sidebar with stats cards, category filters, and regional revenue breakdown
- Dark theme (CARTO Dark Matter)

### Bug fixes and improvements
- Fixed null coordinate crash — 7 Isle of Man/Channel Islands accounts had null lat/lon, which crashed Leaflet and prevented sidebar stats from rendering on initial load
- Added null guard in `renderMarkers()` and wrapped init in `setTimeout` for reliability
- Fixed stale data in region table click handler (now uses `getFiltered()`)

### Category normalisation
- Merged TOY + TOYS → TOYS
- Merged INDEPENDANT, INDEPENTANT, INDEPENDENT → INDEPENDENT
- Merged GARDEN CENTRE variants → GARDEN CENTRE
- Renamed NaN/blank categories → NOT SPECIFIED

### Branding
- Changed title from "Gift Universe" to "Sophie's Accounts — The Source Wholesale"
- Updated subtitle to "Net Revenue by UK Postcode"

### Accounts list
- Added "All Accounts" tab with searchable, sortable list of every account
- Search filters by name, postcode, region, district, or category
- Sort by revenue (high/low) or alphabetical
- Click any account to fly to it on the map and open its popup

### Missing coordinates fixed
- Added manual coordinates for all 7 null accounts: JAC Distribution Ltd, Aladdins Cave, Ransoms Garden Centre, Chaos Ltd, Guernsey Candles Ltd, Extreme Art & Gadgets, Pro Electronics Ltd

### Region assignments fixed
- Harrods (W14 8YW) — Unknown → London
- Ethical Superstore (NE10 8HQ) — Unknown → North East
- Leaping Lizards (N6 5QC) — Unknown → London
- Removed "Unknown" region entirely

### Map restyled
- Switched from dark theme to CARTO Positron (light)
- Added UK region boundaries (9 English regions + Scotland + Wales) from ONS
- Each region colour-coded with hover and click-to-highlight
- Sidebar region table rows show colour dots and highlight when clicked
- Map bounded to UK area (lat 35–62) to hide irrelevant geography
- Labels rendered above region fills for readability

### Region zoom for islands
- Channel Islands, Isle of Man, Gibraltar now zoom to account cluster when clicked in sidebar (no GeoJSON boundary available, falls back to coordinate bounds)

### Gibraltar fixes
- Extended map bounds south to lat 35 so Gibraltar is reachable
- Offset two overlapping Gibraltar accounts so both bubbles are visible

### Bubble colours improved
- Replaced dark/muddy red scale with vibrant gradient: orange → red → purple → indigo
- Colours stay clean and distinct even on large bubbles

### Deployment
- Created public GitHub repo: samuelblakek/the-source-uk-revenue-map
- Enabled GitHub Pages via `gh-pages` branch
- Live at https://samuelblakek.github.io/the-source-uk-revenue-map/
- Raw Excel file excluded from repo via `.gitignore` (contains customer data)
