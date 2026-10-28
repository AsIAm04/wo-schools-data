# wo-schools-data

Data repository for **Beyond the Score**, a West Orange Public Schools performance data tool. Sibling repo to `wo-ledger-data` and `wo-schoolboard-ledger-data`, following the same convention: source data lives here, the frontend (a Squarespace Code Block) fetches it at runtime rather than embedding it inline.

## Files

- **`wops-performance.json`**: NJDOE School Performance Report data for all 11 West Orange Public Schools, school years 2015-16 through 2024-25. Structure:
  - `schools`: school code to school name
  - `years`: ordered list of school-year keys
  - `data[schoolCode][year].subgroup_metrics`: ELA/Math proficiency, chronic absenteeism, out-of-school suspension rate, and 4-year graduation rate, each disaggregated by every subgroup NJDOE publishes for that school-year (small subgroups are suppressed by NJDOE under approximately 10 students; those simply don't appear as a key rather than being estimated)
  - `data[schoolCode][year].flat_metrics`: schoolwide-only figures (economic disadvantage %, disability %, multilingual learner %, race/ethnicity composition, student-teacher ratio, average teacher experience)

- **`wops-neighborhood.json`**: Elementary-school-attendance-zone-level neighborhood context: Census/ACS (income, poverty, education, homeownership), EPA air quality (PM2.5, diesel particulate matter), USDA food access, and BTS/CDC transportation noise. This is a **current single snapshot**, not a year-by-year time series, and covers elementary schools only (middle and high school attendance zones are districtwide rather than address-based, so this crosswalk doesn't apply to them).

- **`wops-home-language.json`**: NJDOE School Performance Reports "Enrollment by Home Language" for the 11 schools, 2015-16 through 2024-25, from each year's school-level database (`Database_SchoolDetail.xlsx`, worksheet `EnrollmentByHomeLanguage`; `LanguageDiversity` in 2015-16), downloaded 2026-10-27. Structure: `years[year][schoolCode]` is a list of `{language, pct}` with NJDOE's labels unchanged (including "Other" through 2023-24 and "Others" in 2024-25). NJDOE lists only the most common languages, so a language missing for a school in a year was not listed, not zero. Each student's primary home language counts, English included, whether or not the student is a Multilingual Learner.

- **`wops-zones.json`**: Approximate elementary attendance zones for the seven elementary schools, plus the district outline, as GeoJSON (WGS84). Built 2026-10-22 from the West Orange Board of Education Street Directory (revised June 5, 2026), drawn on 2020 Census blocks: each street the directory assigns to a single elementary school was matched by name to OpenStreetMap street geometry, every point in the district was given the school of its nearest such street, and each census block then took the school covering most of it. Blocks clearly split between schools were divided along a smoothed version of that assignment, so edges inside those blocks are interpolated. Streets split by house number were left out, so edges near them are approximate. The district outline is the Census township boundary. The file's `metadata` records the method, the cleanup rules and a consistency check. Use the district's street directory, not this file, to confirm an address. Street geometry © OpenStreetMap contributors, ODbL.

## Update cadence

Extend `wops-performance.json` with a new year's entry once NJDOE publishes that year's School Performance Reports. `wops-neighborhood.json` should be refreshed if a newer Census/ACS vintage or EPA air quality dataset becomes available; there's no fixed schedule for this since it's a snapshot, not a trend series.

## Consuming this data

The frontend fetches these files directly via their raw GitHub URLs:

```
https://raw.githubusercontent.com/AsIAm04/wo-schools-data/main/wops-performance.json
https://raw.githubusercontent.com/AsIAm04/wo-schools-data/main/wops-neighborhood.json
```

This repo should be public so `raw.githubusercontent.com` can serve it without authentication, consistent with the other West Orange data repos.
