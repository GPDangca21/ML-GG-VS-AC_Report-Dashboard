# ML-GG-VS-AC_Report-Dashboard

End of day operations dashboard for the Merhoff & Larkin account, covering four brands:
Merhoff & Larkin, Galdor's Guild, Alley Cats and Vent Scent.

Replaces the daily Slack EOD text posts. Same information, one permanent link.

---

## Live link

Once GitHub Pages is on:

```
https://GPDangca21.github.io/ML-GG-VS-AC_Report-Dashboard/
```

### Filtered links

Add `?brands=` to limit which brands the page shows. This locks the whole page,
not just the view, so the hidden brands do not appear in the tabs either.

| Audience | Link |
|---|---|
| Mason + Dylan (ML & GG only) | `.../ML-GG-VS-AC_Report-Dashboard/?brands=ml,gg` |
| Mason only (Alley Cats & Vent Scent) | `.../ML-GG-VS-AC_Report-Dashboard/?brands=ac,vs` |
| Everything | `.../ML-GG-VS-AC_Report-Dashboard/` |

Brand ids: `ml`, `gg`, `ac`, `vs`

This is a URL filter, not authentication. Anyone with the full link sees all four
brands. If the separation needs to be enforced, use two separate repos instead.

---

## Updating the report each day

Everything lives in one file. Open `index.html` and edit the `REPORT` block near the
top of the `<script>` section. Nothing below the "Rendering" comment needs touching.

```js
const REPORT = {
  date: "2026-08-31",        // shown in the header
  shiftIn: "7:00 AM",
  shiftOut: "3:00 PM",
  timezone: "Manila",
  ...
```

### Statuses

Each item carries one status. The status decides its colour, its label, and which
group it rolls into on the Overview chart.

| Status | Label shown | Rolls up as |
|---|---|---|
| `issue` | Issue | Outstanding |
| `moving` | In flight | Outstanding |
| `hold` | On hold | Outstanding |
| `waiting` | Your call | Pending approval |
| `done` | Clear | Accomplished |

### Adding an item

Drop it into the right section's `items` array:

```js
{ brand:"ml", status:"moving", title:"Short task name",
  detail:"One or two sentences of context.",
  meta:{ "ASIN":"B0XXXXXXXX", "Shipment":"FBA1XXXXXXX" },
  link:{ label:"Open the sheet", url:"https://..." } }
```

`detail`, `meta` and `link` are all optional. `brand` and `status` are required.

### Adding a client approval

Items in the `asks` array appear in the Pending approvals panel at the top:

```js
{ brand:"gg", title:"Approve the listing sheet",
  detail:"Why it is blocked and what happens once approved." }
```

If something is in `asks`, give the matching table item the `waiting` status so the
chart counts and the panel agree.

### Counts and charts

Do not maintain any numbers by hand. Every tab count, the legend totals, the bar
chart and the tally line are calculated from the data at page load.

---

## Deploying

1. Commit the edited `index.html`
2. Wait about a minute for Pages to rebuild
3. The link is unchanged — send the same URL every day

---

## Notes

- Single file, no build step, no dependencies. The logo is embedded as base64.
- The only external request is the Archivo webfont from Google Fonts. If that is
  blocked, the page falls back to the system sans-serif and still works.
- The Print button always outputs the complete report regardless of which tab is
  open, so a saved PDF is never a partial record.
- Repo is public, which GitHub Pages requires on the free tier. Supplier names,
  PO numbers, ASINs and shipment IDs are therefore publicly readable. Check this
  is acceptable to the client before the first commit.
