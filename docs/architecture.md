# Architecture — The Source UK Revenue Map

## System Design

This is a single-file static web application. There is no backend, no database, and no build step. The entire application — data, styling, mapping logic, and region boundaries — is embedded in one HTML file.

## Components

### Map Layer (Leaflet.js)
- Base tiles: CARTO Positron (light, no labels) — clean background for data visualisation
- Label tiles: CARTO Positron (labels only) — rendered above regions so place names stay visible
- Pane ordering: regions (z-index 400) → markers (z-index 600) → labels (z-index 650)
- Map bounds: lat 35–62, lon -12–4 (UK + Gibraltar)

### Region Boundaries (GeoJSON)
- 11 regions: 9 English regions (from ONS Regions December 2022 BUC) + Scotland + Wales (from ONS Countries December 2024 BUC)
- Simplified to ~57KB using coordinate rounding (3 decimal places) and polygon filtering
- Each region has a unique fill colour, hover effect, and click-to-highlight behaviour
- Regions without GeoJSON (Channel Islands, Isle of Man, Gibraltar) fall back to zooming to account coordinates

### Account Markers (CircleMarkers)
- 130 accounts plotted by geocoded postcode
- Radius proportional to revenue (sqrt scale, 4–34px)
- Colour scale: grey (£0) → orange (<£1k) → red (<£5k) → purple (<£20k) → indigo (£50k+)
- Popups show: account name, postcode, district, region, net revenue, margin %, units sold, category

### Sidebar (HTML/CSS)
- Two tabs: Overview and All Accounts
- Overview: 4 stat cards, category filter buttons, region table with click-to-highlight
- All Accounts: search box, sort controls, scrollable list with click-to-zoom

## Data Pipeline (build-time)

```
Excel file
    │
    ▼
pandas (read + clean categories + strip postcodes)
    │
    ▼
postcodes.io bulk API (postcode → lat, lon, region, district)
    │
    ▼
Manual fixes (Isle of Man, Channel Islands, Gibraltar coords + Unknown region corrections)
    │
    ▼
ONS ArcGIS API (region boundary GeoJSON → simplify)
    │
    ▼
Embed as JSON literals in HTML file
    │
    ▼
UK_Revenue_Map.html (single file, ~113KB)
```

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| Single HTML file | Zero deployment complexity — just email or share the file |
| Leaflet over Google Maps | Free, no API key, lighter weight |
| CARTO Positron (light) | Professional appearance for business sharing |
| Embedded data (not external JSON) | Works offline (except map tiles), nothing to configure |
| CircleMarkers not regular Markers | Scale cleanly, no image assets, better for dense clusters |
| ONS official boundaries | Authoritative UK region definitions |
