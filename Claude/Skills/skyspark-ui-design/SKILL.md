---
name: skyspark-ui-design
description: >
  Use this skill whenever the user wants to design, describe, or generate UI specifications
  for SkySpark pUb (published views) dashboards and analytics screens. Trigger whenever the
  user mentions SkySpark, pUb views, building analytics dashboards, meter data displays,
  spike filter analysis, demand charts, or wants to translate a screenshot or mockup into a
  SkySpark-compatible UI specification. Also trigger when the user asks to replicate, define,
  or document a dashboard layout for use in SkySpark's pUb framework — even if they don't
  say "skill" explicitly.
---

# SkySpark pUb UI Design Skill

This skill captures the design language and component patterns established in the
**Hot Water Demand — Spike Filter Analysis** dashboard (Jan 1 – Mar 13, 2026) and
translates them into SkySpark pUb UI specifications.

---

## Reference Dashboard: Hot Water Demand — Spike Filter Analysis

### Layout Overview

Single-page, vertically stacked layout with a light neutral background (`#f0f0ec` approx).
No sidebar. All content centered in a max-width container with generous padding.

**Sections (top to bottom):**
1. Page Header
2. KPI Summary Cards (3-column row)
3. Time-Series Chart Panel
4. Filter Settings Panel
5. Footer navigation links

---

## Section Specifications

### 1. Page Header

| Property | Value |
|---|---|
| Title | Bold, large (`~28px`), dark gray (`#1a1a1a`) |
| Subtitle | Small caps label, light gray, pipe-separated metadata fields |
| Date range badge | Pill/badge style, right-aligned, muted background, small text |

**Subtitle fields:** Equipment type · Interval · Location  
**Example:** `Hot Water Meter · 15-min intervals · New York`

---

### 2. KPI Summary Cards

Three equal-width cards in a horizontal row. White background, rounded corners, subtle shadow.

| Card | Label | Value Style | Sub-label |
|---|---|---|---|
| Filtered Peak Demand | `FILTERED PEAK DEMAND` | Large number, accent blue (`#2563eb`), unit below | `kBTU/h` |
| Filter Threshold | `FILTER THRESHOLD` | Large number, dark, unit + formula below | `kBTU/h · 95th pct × 1.10` |
| Readings Adjusted | `READINGS ADJUSTED` | Large number, muted gray | `of {N} total readings ({pct}%)` |

**Card label style:** All-caps, small, letter-spaced, muted gray  
**Value style:** `~48px`, bold  
**Sub-label style:** Small, gray, inline after unit  

---

### 3. Time-Series Chart Panel

White card, rounded corners, full width.

| Property | Value |
|---|---|
| Title | `Demand over time`, left-aligned, medium weight |
| Legend | Right-aligned: gray line = "raw data", blue line = "filtered" |
| X-axis | Datetime labels, `MM-DD HH:MM:SS` format |
| Y-axis | `kBTU/h`, numeric with comma formatting |
| Raw data series | Light gray (`#d1d5db`), thin line, shows spikes at full amplitude |
| Filtered series | Accent blue (`#2563eb`), thin line, capped at threshold |
| Background | White, light gray horizontal grid lines |
| Spike behavior | Raw gray line spikes to actual values (e.g., 7,000); blue filtered line stays near baseline |

---

### 4. Filter Settings Panel

White card, rounded corners, full width. Two-column grid layout for inputs.

**Panel title:** `Filter settings`, medium weight, left-aligned

**Left column inputs:**

| Field | Label | Type | Default | Unit |
|---|---|---|---|---|
| Percentile Baseline | `PERCENTILE BASELINE` | Number input | `95` | `th percentile` |
| Min Spike Duration | `MIN SPIKE DURATION TO FILTER` | Number input | `60` | `minutes` |

**Right column inputs:**

| Field | Label | Type | Default | Unit |
|---|---|---|---|---|
| Variation Factor | `VARIATION FACTOR` | Number input | `10` | `% above cap` |
| Absolute Floor | `ABSOLUTE FLOOR` | Number input | `150` | `kBTU/h` |

**Input style:** Rounded border, left-padded text, short width (`~100px`)  
**Label style:** All-caps, small, letter-spaced, muted gray above each input  

**Apply Button:**  
- Solid accent blue background (`#2563eb`)  
- White text, rounded corners  
- Positioned below left column inputs  

**Threshold Formula Explanation (below button):**  
Small gray text showing the computed threshold formula:  
> `Threshold calculated as: 95th percentile (64.1 kBTU/h) × 1.10 = 150 kBTU/h. Spikes shorter than 60 minutes are replaced with the local rolling average.`  
Bold inline values for key computed figures.

---

### 5. Footer Navigation

Centered, small text, muted gray.  
Pipe-separated links: `Building Analytics · Hot Water Demand Spike Filter · Data quality report`

---

## Design Tokens

| Token | Value |
|---|---|
| Background | `#f0f0ec` (warm off-white) |
| Card background | `#ffffff` |
| Card border radius | `12px` |
| Card shadow | `0 1px 4px rgba(0,0,0,0.08)` |
| Primary accent | `#2563eb` (blue) |
| Text primary | `#1a1a1a` |
| Text secondary | `#6b7280` |
| Label / caps text | `#9ca3af`, `font-size: 11px`, `letter-spacing: 0.08em` |
| Border color | `#e5e7eb` |
| Grid line color | `#f3f4f6` |

---

## SkySpark pUb Implementation Notes

- Use `pUb` view type with `axonFuncDef` for threshold calculation logic.
- Chart component: use `chart` pUb widget with dual series (raw + filtered).
- KPI cards: use `text` or `kpi` widgets bound to Axon expressions.
- Filter inputs: bind to `pUb` form controls with `onChange` handlers triggering recalculation.
- Date range: pass as pUb query parameters (`start`, `end`), displayed in header badge.
- Equipment/location metadata: resolve via `navPathStr()` or `dis()` on the point/equip record.

---

## Usage Instructions for Claude

When a user asks to design or replicate a SkySpark pUb UI:

1. **Identify the data type** — meter type, interval, units, site.
2. **Map KPIs** — determine the 2–3 headline metrics for the card row.
3. **Define chart series** — raw vs. processed/filtered, axis units, time range.
4. **Specify filter controls** — list configurable parameters and their defaults.
5. **Output format** — produce either:
   - A written UI specification (following this document's structure), or
   - An HTML/React prototype rendered inline using the design tokens above.

Always maintain the established visual hierarchy: header → KPI cards → chart → controls → footer.
