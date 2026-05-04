# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A self-contained single-file web app (`index.html`) for University of Sheffield staff to calculate whether they are owed additional annual leave in lieu of public holidays and university closed days. No build step, no dependencies, no server — everything runs in the browser.

## Deploying

Changes pushed to `main` are automatically deployed via GitHub Pages to:
`https://sheffield-mps.github.io/closed-day-calculator/`

There is no build or compilation step — just push and GitHub Pages serves the file directly.

## Architecture

Everything lives in `index.html`: HTML structure, CSS styles, and JavaScript in a single `<script>` block. The key sections of the script are clearly delimited with comments:

- **State** — `leaveYearStart`, `rotationMode`, `closedDays`, `cachedBankHolidays`
- **Leave year** — Oct 1 – Sep 30 range; `leaveYearRange()` and `currentLeaveYearStart()`
- **Bank holidays** — fetched once from `https://www.gov.uk/bank-holidays.json` and cached in `cachedBankHolidays`; falls back to Dec 27–31 only if the fetch fails
- **Closed days** — bank holidays + university Dec 27–31 closure, user-editable
- **Rotation** — `rotationMode` is `'one'` or `'two'`; two-week rotation uses `getRotation()` which calculates elapsed weeks in milliseconds from a reference Monday (avoiding the 53-week year bug that affects `WEEKNUM()`-based approaches)
- **Calculation** — `recalculate()` computes fair entitlement (FTE × 7hrs × number of closed weekdays) vs actual benefit (contracted hours on each closed weekday), rounds difference to nearest 0.5 day

## Key domain rules

- Leave year: **1 Oct – 30 Sep** (not calendar year, not Sep–Aug)
- Full-time hours: **35 hrs/week** (`FT_HOURS` constant)
- University always closed **27–31 Dec** (pre-populated, weekends excluded automatically)
- Result rounded to nearest **0.5 day** using `Math.ceil(x * 2) / 2`
- Only **weekdays** (Mon–Fri) count as closed days
- Two-week rotation reference date: **Monday of the week containing 1 Oct** of the leave year start (pre-populated, editable)
- Bank holidays are **England & Wales** only
