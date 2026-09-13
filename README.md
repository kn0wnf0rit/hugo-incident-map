# Hugo Incident Map (2026)

Interactive map of every incident the Washington County Sheriff's Office logged with a Hugo, Minnesota address in 2026, colored by severity. Hover a dot for a summary; click for the full public record.

**Live map:** see the GitHub Pages URL in the repo settings (root `index.html`).

## Data

- Source: Washington County Sheriff's Office weekly media incident summaries — https://web1.co.washington.mn.us/MediaReports/RMS/ (`IncidentSummary_YYYYMMDD.csv`, one file per week).
- Filter: rows with city `HUGO` and a 2026 timestamp (2,432 incidents, Jan 1 – Sep 5, 2026 as of the Sept 9 file).
- Fields published by the county: agency, city, timestamp, case number, block-level address or intersection, event description. There is no narrative text in the public log.

## How the dots are placed

The county only publishes block-level addresses ("5XXX 157th St N") or intersections, so every position is approximate by design:

- Streets, lakes and the city boundary come from OpenStreetMap (© OpenStreetMap contributors, ODbL).
- Block addresses are placed along the named street using Washington County's house-number grid (about 1,000 numbers per mile; 4-digit numbers run east–west, 5-digit numbers run north–south), calibrated against OSM address points to roughly ±40 m.
- Intersections are computed from the street centerlines.
- Co-located calls are nudged a few dozen meters apart so they stay visible.
- 64 incidents with no usable address ("Unknown") are not shown.

Severity tiers (Critical / High / Medium / Low) are an editorial grouping of the county's 120 event labels, not an official ranking.

## Files

- `index.html` — the whole map: page, data and map geometry in one self-contained file (only the web fonts load externally).
