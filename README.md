# wo-schools-data

Data repository for **Beyond the Score**, a West Orange Public Schools performance data tool. Sibling repo to `wo-ledger-data` and `wo-schoolboard-ledger-data`, following the same convention: source data lives here, the frontend (a Squarespace Code Block) fetches it at runtime rather than embedding it inline.

## Files

- **`wops-performance.json`** — NJDOE School Performance Report data for all 11 West Orange Public Schools, school years 2015-16 through 2024-25. Structure:
  - `schools`: school code to school name
  - `years`: ordered list of school-year keys
  - `data[schoolCode][year].subgroup_metrics`: ELA/Math proficiency, chronic absenteeism, out-of-school suspension rate, and 4-year graduation rate, each disaggregated by every subgroup NJDOE publishes for that school-year (small subgroups are suppressed by NJDOE under approximately 10 students; those simply don't appear as a key rather than being estimated)
  - `data[schoolCode][year].flat_metrics`: schoolwide-only figures (economic disadvantage %, disability %, multilingual learner %, race/ethnicity composition, student-teacher ratio, average teacher experience)

- **`wops-neighborhood.json`** — Elementary-school-attendance-zone-level neighborhood context: Census/ACS (income, poverty, education, homeownership), EPA air quality (PM2.5, diesel particulate matter), USDA food access, and BTS/CDC transportation noise. This is a **current single snapshot**, not a year-by-year time series, and covers elementary schools only (middle and high school attendance zones are districtwide rather than address-based, so this crosswalk doesn't apply to them).

## Update cadence

Extend `wops-performance.json` with a new year's entry once NJDOE publishes that year's School Performance Reports. `wops-neighborhood.json` should be refreshed if a newer Census/ACS vintage or EPA air quality dataset becomes available; there's no fixed schedule for this since it's a snapshot, not a trend series.

## Consuming this data

The frontend fetches these files directly via their raw GitHub URLs:

```
https://raw.githubusercontent.com/AsIAm04/wo-schools-data/main/wops-performance.json
https://raw.githubusercontent.com/AsIAm04/wo-schools-data/main/wops-neighborhood.json
```

This repo should be public so `raw.githubusercontent.com` can serve it without authentication, consistent with the other West Orange data repos.
