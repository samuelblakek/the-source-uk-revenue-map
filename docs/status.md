# Status

## Current: v1.0 — Complete ✓

The interactive UK revenue map is finished and ready to share with the team.

**Deliverable:** `output/UK_Revenue_Map.html` — open in any browser.

**Live URL:** https://samuelblakek.github.io/the-source-uk-revenue-map/

## What was delivered

- 130 accounts plotted on an interactive UK map with revenue-proportional bubbles
- 11 UK regions with colour-coded boundaries and click-to-highlight
- Sidebar with summary stats, category filters, and regional revenue/customer breakdown
- Searchable, sortable accounts list with click-to-zoom
- All data self-contained in a single 113KB HTML file

## Data coverage

- 130/133 accounts plotted (3 excluded: 1 Bermuda postcode with no coordinates, 2 had issues resolved)
- All 7 Isle of Man + Channel Islands accounts manually geocoded
- 2 Gibraltar accounts included (map extends south to accommodate)
- 0 accounts in "Unknown" region

## Picking this up again

To refresh the map with new data, drop a fresh Excel export into `data/` — the original is excluded from the repo for customer privacy. Everything else (docs, code, hosting) is self-contained and ready to go.

## Potential future enhancements

- Refresh with updated data (re-run data pipeline with new Excel export)
- Add year-on-year comparison (Net 1 vs Net 2 toggle)
- Add margin % as an alternative bubble sizing metric
- Export region summary as PDF/Excel for offline reporting
- Add Northern Ireland boundary if accounts are added there
- Heat map overlay as an alternative to bubbles
