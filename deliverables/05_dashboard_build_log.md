# Prompt 5. Build Log — Six-Case Analytics Dashboard

**Scope note:** the assignment as written asks to pick *one* case for this dashboard. At the person's explicit
request, this was expanded to cover **all six cases** in a single self-contained file, with a case selector as
the primary control. This is a deliberate deviation from the written spec, done on request; each case still
meets the per-case minimums (KPI header, 3+ tabs, 5+ charts, a working filter, a Data Caveats panel) rather than
sharing one generic set of charts across all six.

**Case chosen for the original single-case version: F, marketing_campaigns.csv** (kept as one of the six
options). Reason: Prompt 4 had already surfaced a genuinely misleading headline number (Email's 88.8x ROAS),
which made a natural centerpiece for a Data Caveats panel.

**Turns it took: 9**

1. **Initial ask (single-case version)** — self-contained HTML, no external libraries, KPI header, 3+ tabs,
   channel dropdown filter, charts for revenue/spend/ROAS/CPA by channel plus a monthly trend, for marketing only.
2. **First build** — raw SVG chart helpers (bar, line, funnel) and the channel filter.
3. **Bug found and fixed (marketing ROAS chart)** — see below, item 1.
4. **Added funnel chart + summary table**, then the **Data Caveats tab** for the marketing case.
5. **Verification pass on the single-case version** — `node --check` on the extracted script, then a full
   `jsdom` run exercising both filters and all four tabs, plus a `wkhtmltoimage` screenshot as a visual check.
6. **Scope change to all six cases** — restructured the page around a Case selector that swaps the entire
   dataset, KPI set, chart contents, and caveats panel, reusing the same generic `barChart` / `lineChart` /
   `funnelChart` primitives so all six cases share one rendering engine instead of six separate ones.
7. **Built out each case's Overview/Detail/Trends/Caveats content** (cafe, support, reviews, loans, invoices,
   marketing), keeping each case's own filter (daypart, category, verified purchase, score band, status,
   channel) and its own Data Caveats content, including the real fairness flag for the loans case and explicit
   "not applicable" fairness notes for the other five.
8. **Bug found and fixed (support tickets average)** — see below, item 2.
9. **Full verification pass across all six cases** — `node --check` again, then a `jsdom` run that switches the
   Case selector through all six values, checks that every KPI card, all six chart containers, and the detail
   table populate with real (non-NaN, non-empty) content for each case, and exercises each case's first filter
   option to confirm the numbers actually change.

**Two bugs the AI introduced, and how they were fixed**

1. **Marketing ROAS chart, linear-scale bug.** The first version of the ROAS bar chart used the same
   linear-scale bar function as the revenue and spend charts. Because Email's ROAS is 88.8x against Google Ads'
   1.44x, Meta's 0.99x, and LinkedIn's 0.55x, the linear scale rendered every bar except Email's as a 1-2 pixel
   sliver, invisible and unreadable. Fixed by adding a `log` option to the bar-chart function that scales bar
   height by `log10(value)`, used only for this chart, with the chart subtitle explicitly stating a log scale is
   in use and why.
2. **Support tickets, NaN average from unresolved tickets.** The "Avg resolution (hrs)" KPI and the "resolution
   hours by priority" chart both came out as `NaN` for every value. Cause: 132 of the 600 tickets are still
   `open` or `pending` and have no `resolution_hours` value at all (`null`), and summing a list that includes
   `null` in JavaScript propagates `NaN` through the whole average. Caught during the `jsdom` verification pass,
   which prints each case's KPI values and flagged `NaN` in the support case immediately. Fixed by filtering to
   only tickets with a real `resolution_hours` value before averaging, in both the KPI and the chart, and by
   adding the resolved count (e.g. "23.6 (n=468)") to the KPI label so the exclusion is visible rather than
   silent, plus a line in that case's Data Caveats panel stating the 132 excluded tickets explicitly.

**Verification method, all six cases**

Beyond visual inspection, every case was checked programmatically: the script was extracted and run through
`node --check` for syntax, then the full page was loaded in `jsdom` (a headless DOM implementation) with the
Case selector cycled through all six values. For each case this confirmed: all five KPI cards render a real,
finite value (not `NaN` or `undefined`); all six chart `<svg>` containers receive a non-trivial number of drawn
elements; the detail table populates with rows; and changing that case's first filter actually changes the KPI
numbers (confirming the filter is wired up, not just present in the DOM).

**What I'd still flag to a stakeholder before they trust this dashboard**

Each case's own Data Caveats tab covers its specific limits (sampling windows, the marketing ROAS illusion, the
loans fairness flag). Across all six: the six datasets have different, non-overlapping time windows (per the
Prompt 1 orientation note), so nothing on this dashboard should be read as a cross-case comparison without
checking the date ranges first.

