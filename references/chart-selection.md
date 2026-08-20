# Chart selection

Pick the chart from the question the metric answers, not from what looks impressive.
This template supports four types on purpose. Most real dashboards need only these.

| The question | Chart | Tile `type` | Notes |
|---|---|---|---|
| How much / how many right now? | Big number | `kpi` | One value, optional delta vs. prior period. The workhorse tile. |
| How does it compare across categories? | Bar | `bar` | Ranking, by-segment, by-plan. Sort descending unless order is meaningful (e.g. stages). |
| How is it changing over time? | Line | `line` | Trends, run-rate, cohorts over weeks/months. Needs 3+ points to be worth it. |
| What are the parts of a whole? | Donut | `donut` | Only for 5 slices or fewer, and only when shares (not absolute values) are the point. Otherwise use a bar. |

## Rules of thumb
- **Default to `kpi` and `bar`.** They carry most dashboards. Reach for `line` only when time is the story.
- **A single number beats a one-bar bar chart.** If a "comparison" has one item, it's a KPI.
- **Donuts are easy to misread.** Never use one for more than 5 slices, and never to compare two donuts to each other. When in doubt, bar.
- **Add a delta to KPIs** whenever a prior period exists. "128.4K, up 6.2%" answers "and is that good?" in the same glance.
- **Match direction to meaning.** For "lower is better" metrics (churn, cost, latency), say so in the label so a down arrow doesn't read as bad news.

## What this template does NOT draw
Scatter, histogram, heatmap, and geo/maps are out of scope by design. If a metric truly
needs one, note it and hand the user a focused follow-up rather than forcing it into a
donut. Keeping the type list short is what keeps the dashboards readable.
