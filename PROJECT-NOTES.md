# Travel London — Project Notes

## Vision
First layer in a "map of London" that surfaces sellable datapoints. Long-term goal:
predict traffic bottlenecks, pollution build-ups, roadworks impact, and overlay company/
population data. **Honest sequence: display live layers → store them over time → then
predict.** Prediction needs collected history first.

## Where we got to (12 June 2026)
Built and shipped Layer 1 — population density — as a live, public website on free
infrastructure (zero running cost).

- **Live URL:** https://rspiegelberg-cmd.github.io/Travel-London/ (GitHub Pages)
- **Repo:** https://github.com/rspiegelberg-cmd/Travel-London (public)
- **Pages built:**
  - `index.html` — 2D density map, 4,994 LSOA areas (the granular view)
  - `3d.html` — 3D view with three modes: density blocks · true buildings · density-scaled buildings
  - `boroughs.html` — simpler 33-borough overview
- **Data:** Census 2021 TS006 population density (ONS), stored as `data/lsoa-density.js`.
  Boundaries fetched live from ONS Open Geography ArcGIS server. Borough populations = ONS mid-2024.
- **Tech:** plain HTML + Leaflet (2D) and MapLibre GL (3D). No server, no build step, no cost.
- Raw census zip + extracted CSVs are git-ignored (re-downloadable; only the clean data file is committed).

## Layer 2 — TfL roadworks & disruptions (13 June 2026)
Added the first **live** layer, on `index.html`, with a layer-toggle control (top-left).

- **Feed:** TfL Unified API `https://api.tfl.gov.uk/Road/all/Disruption` — keyless, free, ~85
  active items. Fetched live in the browser each page load (no storage yet).
- **Split into two toggleable overlays:** "Planned roadworks" (category `Works`, orange dots)
  and "Live disruptions" (incidents/delays/asset issues/events, red dots). Population density
  is now also a toggleable overlay, so all three switch on/off independently.
- **Markers:** colour by type, sized by severity; popup shows location, severity, description,
  current update, and start→end dates. Panel shows a live count.
- All additions are self-contained in `index.html`; the density rendering is untouched.

Also added the same live layers to **`3d.html`** (13 June 2026): TfL planned roadworks +
live disruptions as MapLibre circle layers, with two independent toggle checkboxes ("Live
layers" section in the panel) and click popups. Sits on top of any 3D mode (density blocks /
buildings / scaled). Existing 3D modes untouched.

## Next up (recommended order)
1. **Start storing daily snapshots** of the TfL feed to the repo — the raw history that future
   prediction work depends on (e.g. a small JSON saved per day).
2. **Add a pollution layer** (e.g. London Air / Breathe London) as the next live feed.
3. **(Optional) put roadworks on `boroughs.html`** too, for full consistency.

## Parked (deliberately)
- Company data (Companies House — needs API key, address geocoding). Better as a later layer.
- "Illegal shops" / inferred data — no clean dataset, legal risk. Avoid.

## Workflow reminder
Claude edits files; Rupert saves via GitHub Desktop (Commit → Push). Pages auto-updates from
the `main` branch within a minute or two of each push.
