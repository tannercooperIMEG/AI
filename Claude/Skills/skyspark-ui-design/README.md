# skyspark-ui-design

A Claude Skill that captures the design language and component patterns for SkySpark **pUb (published views)** dashboards, enabling Claude to generate accurate UI specifications from screenshots, mockups, or verbal descriptions.

---

## Overview

This skill was built from a reference dashboard — **Hot Water Demand — Spike Filter Analysis** — and encodes its layout structure, component definitions, design tokens, and SkySpark pUb implementation notes into a reusable specification Claude can apply to new dashboards.

Once installed, Claude will automatically apply this skill whenever you ask it to design, describe, or replicate a SkySpark pUb UI.

---

## What This Skill Does

- Translates screenshots or mockups into structured SkySpark pUb UI specifications
- Maintains consistent design language across dashboard views
- Documents component-level field definitions, defaults, and units
- Provides SkySpark pUb implementation guidance (Axon bindings, widget types, query parameters)
- Generates HTML/React prototypes using the established design tokens

---

## Trigger Conditions

Claude will invoke this skill when you mention:

- SkySpark or pUb views
- Building analytics dashboards
- Meter data displays or demand charts
- Spike filter analysis
- Translating a screenshot or mockup into a SkySpark UI spec

---

## Reference Dashboard

**Hot Water Demand — Spike Filter Analysis**  
Hot Water Meter · 15-min intervals · New York · Jan 1 – Mar 13, 2026

### Layout Structure

```
┌─────────────────────────────────────────────┐
│  Page Header                    [Date Badge] │
├──────────────┬──────────────┬───────────────┤
│  KPI Card 1  │  KPI Card 2  │  KPI Card 3   │
├─────────────────────────────────────────────┤
│  Time-Series Chart (raw + filtered)         │
├─────────────────────────────────────────────┤
│  Filter Settings Panel                      │
├─────────────────────────────────────────────┤
│  Footer Navigation Links                    │
└─────────────────────────────────────────────┘
```

---

## Design Tokens

| Token | Value |
|---|---|
| Page background | `#f0f0ec` |
| Card background | `#ffffff` |
| Card border radius | `12px` |
| Card shadow | `0 1px 4px rgba(0,0,0,0.08)` |
| Primary accent (blue) | `#2563eb` |
| Text primary | `#1a1a1a` |
| Text secondary | `#6b7280` |
| Label / caps text | `#9ca3af` · `11px` · `0.08em` letter-spacing |
| Border color | `#e5e7eb` |
| Chart grid lines | `#f3f4f6` |
| Raw data series | `#d1d5db` (light gray) |
| Filtered series | `#2563eb` (accent blue) |

---

## Component Reference

### KPI Cards
Three equal-width cards in a horizontal row. Each card contains:
- An all-caps, letter-spaced label in muted gray
- A large (`~48px`) bold numeric value
- A small sub-label with unit and formula context

### Time-Series Chart
Dual-series line chart overlaying raw and filtered data. Raw data is rendered in light gray at full amplitude to expose spikes; the filtered series is rendered in accent blue, capped at the computed threshold.

### Filter Settings Panel
Two-column input grid with number fields for:
- Percentile Baseline (default: 95th)
- Min Spike Duration to Filter (default: 60 min)
- Variation Factor (default: 10% above cap)
- Absolute Floor (default: 150 kBTU/h)

Includes an **Apply** button and a computed threshold explanation rendered as inline bold text.

---

## SkySpark pUb Implementation Notes

| Concern | Approach |
|---|---|
| View type | `pUb` |
| Threshold logic | `axonFuncDef` |
| Chart widget | `chart` with dual series (raw + filtered) |
| KPI widgets | `text` or `kpi` bound to Axon expressions |
| Filter inputs | `pUb` form controls with `onChange` recalculation |
| Date range | pUb query params (`start`, `end`), shown in header badge |
| Equipment/location | `navPathStr()` or `dis()` on point/equip record |

---

## File Structure

```
skyspark-ui-design/
├── SKILL.md       # Claude skill definition and component specifications
└── README.md      # This file
```

---

## Installation

1. Download `skyspark-ui-design.skill`
2. In Claude, go to **Settings → Skills**
3. Upload the `.skill` file
4. Claude will apply this skill automatically for relevant SkySpark UI requests

---

## Extending This Skill

To adapt this skill for additional dashboard types (e.g., chilled water, electrical demand, steam):

1. Edit `SKILL.md` and add a new `## Reference Dashboard` section
2. Follow the same structure: Layout Overview → Section Specifications → Implementation Notes
3. Repackage and reinstall the `.skill` file

---

## License

Internal use. Maintain version history in Git to track dashboard design evolution.
