# Closed Day Calculator

A simple web app for University of Sheffield staff to calculate whether they are owed additional annual leave in lieu of public holidays and university closed days.

Hosted at: https://sheffield-mps.github.io/closed-day-calculator/

## What it does

UK employment law requires that part-time staff receive a fair pro-rata share of public holidays and closed days, regardless of which days of the week those holidays fall on. For example, a member of staff who doesn't work Mondays will never benefit from a Monday bank holiday, so they should receive equivalent time back as additional annual leave.

This tool calculates whether a member of staff is owed extra leave, or has over-benefited, based on:

- Their contracted hours per day
- Whether they work a one-week or two-week rotation
- The bank holidays and university closed days falling within the current leave year (1 Oct – 30 Sep)

The university's enforced closure between 27–31 December is pre-populated automatically each year.

## If bank holiday dates stop appearing

The app fetches England & Wales bank holiday dates live from the UK Government's public API:

```
https://www.gov.uk/bank-holidays.json
```

If bank holidays stop showing (and only the December closed days appear), it most likely means this API URL has changed. To fix it:

1. Open `index.html` in a text editor
2. Go to **line 657** and find:
   ```javascript
   const res = await fetch('https://www.gov.uk/bank-holidays.json');
   ```
3. Visit [https://www.gov.uk/bank-holidays](https://www.gov.uk/bank-holidays) in a browser to find the correct current API URL, and update line 657 accordingly
4. Commit and push the change — GitHub Pages will redeploy automatically within a minute or two

## Repo structure

```
index.html   — the entire application (self-contained, no build step)
README.md    — this file
```
