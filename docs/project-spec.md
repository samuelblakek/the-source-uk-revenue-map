# Project Spec — The Source UK Revenue Map

## Product Requirements

### Who is it for?
Sophie Erwood and the wholesale sales team at The Source Wholesale. Non-technical users who need to visually understand their account distribution across the UK.

### What problems does it solve?
- The team had raw Excel data (account names, postcodes, revenue) but no way to see the geographic picture
- No visibility into which UK regions are strong vs underserved
- Previously attempted with ChatGPT but couldn't produce a working map
- Need to identify gaps for prospecting and territory planning

### User interactions / workflows
1. **Open the map** — double-click the HTML file, opens in any browser
2. **Browse overview** — see all accounts plotted on UK map with revenue bubbles, summary stats, and regional breakdown in sidebar
3. **Filter by category** — click category buttons (Toys, Independent, Attractions, etc.) to isolate account types
4. **Explore regions** — click region names in sidebar or on map to highlight and zoom. See account count and total revenue per region
5. **Find specific accounts** — switch to "All Accounts" tab, search by name/postcode/region, sort by revenue or alphabetically. Click any account to fly to it on the map
6. **Click a bubble** — popup shows account name, postcode, district, region, net revenue, margin %, units sold, category

## Engineering Requirements

### Tech stack
- **Frontend:** Single HTML file with inline CSS and JavaScript
- **Mapping:** Leaflet.js 1.9.4 (loaded from CDN)
- **Map tiles:** CARTO Positron (light_nolabels + light_only_labels)
- **Region boundaries:** GeoJSON from ONS ArcGIS API (9 English regions + Scotland + Wales)
- **Geocoding:** postcodes.io bulk API (free, no key required)
- **Data processing:** Python 3 with pandas (build-time only, not needed to view the map)

### Data sources
- `SOPHIE ACCOUNTS BY TERRITORY.xlsx` — 133 accounts with postcode, Net (1) revenue, margin, quantity, category
- postcodes.io API — UK postcode to lat/lon + region lookup
- ONS Open Geography Portal — official UK region boundary polygons
- Manual coordinates for Isle of Man (54.15°N, 4.48°W), Channel Islands (49.2°N, 2.1°W), Gibraltar (36.14°N, 5.35°W)

### Data quality issues addressed
- 7 accounts had null coordinates (Isle of Man, Channel Islands) — manually geocoded
- 3 accounts assigned "Unknown" region — corrected to London (Harrods, Leaping Lizards) and North East (Ethical Superstore)
- Category spelling inconsistencies: TOY/TOYS → TOYS, INDEPENDANT/INDEPENTANT/INDEPENDENT → INDEPENDENT
- NaN/blank categories → NOT SPECIFIED
- 2 Gibraltar accounts shared identical coordinates — offset for visibility
- Leading spaces in some postcodes — stripped

### Performance
- Final HTML file: ~113KB (all data + GeoJSON embedded)
- Region GeoJSON simplified from 552KB to 57KB
- 130 accounts rendered as Leaflet CircleMarkers (lightweight, no image assets)
