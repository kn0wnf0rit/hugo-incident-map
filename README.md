# Washington County Incident Map

An interactive map of every incident in the Washington County (Minnesota) Sheriff's Office public media log — 242,000+ calls from September 2022 through the latest weekly file — for all 31 cities and townships in the county. Pick a city, filter by year, month, severity or event type, hover a dot for a summary and click for the full record. The county overview shades each city by incidents per year per square mile.

Live: open `index.html` from a web server (GitHub Pages works as-is). It will not run from a `file://` URL because the city data is loaded on demand with `fetch`.

## Layout

```
index.html              the app (all CSS/JS inline; embeds data/index.json)
data/index.json         city list, per-year / per-severity counts, bounding boxes
data/county.json        city polygons + major roads + lakes for the overview
data/city/<slug>.json   per-city basemap (OSM roads, water, streams, rail, boundary, labels)
data/inc/<slug>.json    per-city incidents (compact rows)
```

Incident rows are `[case, YYMMDDHHMM, addressIdx, eventIdx, severity, precisionIdx, lat*1e5, lon*1e5, agencyIdx]` with lookup tables `ad`, `ev`, `ag` in the same file. Precision codes: block, block~, intersection, intersection~, grid, street, area.

## Source and method

* Data: https://web1.co.washington.mn.us/MediaReports/RMS/ — every weekly `IncidentSummary_YYYYMMDD.csv`, concatenated and de-duplicated by case number. The log covers the Sheriff's Office and the police departments that share its records system (Oakdale, Woodbury, Stillwater, Cottage Grove, Bayport, Forest Lake, St. Paul Park). 2022 is partial (starts Sept 4). Rows tagged "Saint Paul" (160) are outside the county and were dropped.
* Placement: the county publishes only a block ("5XXX 157th St N"), an intersection, or a bare street. Each address is matched to OpenStreetMap centerlines restricted to the city (plus a 1 km buffer). Block numbers are placed from OSM address points on that street when they exist, otherwise from Washington County's house-number grid (about 1,000 numbers per mile, with a per-city offset calibrated on OSM address points). Intersections are computed from the centerlines. Older river towns with their own numbering (Stillwater, Bayport, Mahtomedi, Newport, Forest Lake downtown…) fall back to street level more often. Every dot is nudged a few dozen meters (seeded by case number) so overlapping calls stay visible. 97.7% of incidents are placed; the rest ("Unknown", private roads, unmapped streets) are counted but not drawn.
* Severity tiers are an editorial grouping of the county's 1,185 event labels (explicit lists plus keyword rules), not an official ranking.
* Base map © OpenStreetMap contributors (ODbL). Incident data © Washington County.

## Refreshing

The county posts a new weekly CSV every Wednesday. Rebuild = download new files → `clean.py` (dedupe, severity) → `geocode_county.py` → `build_site.py`. Only `data/inc/*.json` and `data/index.json` change when new incidents arrive; basemaps only change if OSM is re-pulled.
