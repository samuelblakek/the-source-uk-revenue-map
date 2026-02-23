# The Source Wholesale — UK Revenue Map

## Project Goal

Interactive HTML map visualising Sophie's independent account revenue (Net 1) by UK postcode for The Source Wholesale. Helps the sales team identify geographic strengths and gaps at a glance.

## Architecture

Single self-contained HTML file using Leaflet.js for mapping, with all data embedded inline. No backend, no build step, no dependencies beyond a browser and internet connection (for map tiles).

See `docs/architecture.md` for full details.

## Hosting

- **Live URL:** https://samuelblakek.github.io/the-source-uk-revenue-map/
- **Repo:** https://github.com/samuelblakek/the-source-uk-revenue-map
- GitHub Pages serves from the `gh-pages` branch (contains only `index.html` and `.nojekyll`)
- To redeploy after updating the map: copy `output/UK_Revenue_Map.html` to `index.html`, switch to `gh-pages`, commit, and push

## Key Files

```
the_source_uk_revenue_map/
├── CLAUDE.md                          # This file
├── index.html                         # Copy of map for GitHub Pages (root)
├── .nojekyll                          # Skips Jekyll processing on Pages
├── .gitignore                         # Excludes raw Excel + .DS_Store
├── data/
│   ├── SOPHIE ACCOUNTS BY TERRITORY.xlsx  # Raw source data (gitignored)
│   └── uk_regions.geojson             # Simplified ONS region boundaries
├── output/
│   └── UK_Revenue_Map.html            # Final deliverable (canonical copy)
├── docs/
│   ├── project-spec.md                # Product & engineering requirements
│   ├── architecture.md                # System design & data pipeline
│   ├── changelog.md                   # Running log of changes
│   └── status.md                      # Current status & next steps
└── tasks/
    └── todo.md                        # Task tracking
```

## Data Pipeline

1. Source Excel → pandas (read & clean)
2. Postcodes → postcodes.io API (geocode to lat/lon + region)
3. Manual fixes for Isle of Man, Channel Islands, Gibraltar coordinates
4. Region assignment corrections (3 "Unknown" accounts fixed)
5. Category normalisation (TOY/TOYS merged, INDEPEND* merged, NaN → Not Specified)
6. ONS ArcGIS API → GeoJSON region boundaries (simplified to 57KB)
7. All data embedded as JSON into a single HTML file

## Constraints

- Must work as a single HTML file (no server, no build tools)
- Users are non-technical — UI must be intuitive
- Map tiles require internet connection (CARTO Positron)
- Postcodes.io doesn't cover Isle of Man, Channel Islands, or Gibraltar — these use manual coordinates
- Category data in source has inconsistent casing/spelling — normalisation required

## Design Decisions

- Light map theme (not dark) — better for sharing in business context
- Map bounded to UK + Gibraltar to avoid irrelevant geography
- Region boundaries from ONS (official), simplified for file size
- Bubble colour scale: warm orange → red → purple → indigo (stays vibrant at all sizes)
- Revenue shown in popups as individual account data, region table shows aggregated totals
