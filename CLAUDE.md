# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page poker preflop range viewer (簡易プリフロップレンジビューア) built in vanilla HTML/CSS/JavaScript with no dependencies, no build step, and no package manager. Open `index.html` directly in a browser to run it.

## Running the App

```bash
# Open directly in a browser (no server needed)
open index.html          # macOS
xdg-open index.html      # Linux
```

There are no build, lint, or test commands — the entire application is one self-contained file.

## Architecture

Everything lives in `index.html` (~764 lines), organized into three embedded sections:

**CSS (lines 12–206)**
- Dark theme with a 13×13 grid layout for hand categories
- 9-bucket color system (buckets 0–9) mapping hand strength to colors: black (out of range) → purple → red → orange → yellow → dark green → light green → cyan → white → gray

**HTML (lines 210–335)**
- Two independent range sections: Hero and Villain
- Each section has a 13×13 grid container and a row of checkboxes to toggle visibility per color bucket
- A third section handles PokerStars hand history (HH) import and analysis

**JavaScript (lines 337–761)**
- `strengthMapHero` / `strengthMapVillain` — plain objects mapping hand notation to bucket numbers (0–9). These are the primary data to edit when changing ranges.
- `buildGrid(containerId, strengthMap)` — generates the interactive 13×13 grid for a given range map. Called once on page load for both hero and villain.
- Cell click: toggles individual hand visibility. Row/column header click: highlights the full row or column.
- Checkbox filters: toggling a bucket checkbox hides all cells assigned that bucket number.
- `parseHandsFromText(text)` — parses PokerStars hand history format; extracts hole cards and pot results.
- `calcNetChipsForHero(handText)` — calculates net profit/loss in BB units for a single hand.
- Results are capped at 30 hands to avoid UI overload.

## Hand Notation Convention

| Format | Meaning          | Example |
|--------|-----------------|---------|
| `AA`   | Pocket pair      | `KK`    |
| `AKs`  | Suited combo     | `QJs`   |
| `AKo`  | Offsuit combo    | `T9o`   |

The 13×13 grid follows standard poker hand matrix layout: pairs on the diagonal, suited hands above-right, offsuit hands below-left.
