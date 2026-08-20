---
name: api-dashboard-builder
description: Turns the APIs a team already has into a clean, live dashboard. Use when someone wants to build a dashboard, visualize metrics, track KPIs, or "see all our numbers in one place" and has API endpoints (or an OpenAPI/Swagger spec, or API docs) to pull from. Asks the fewest questions it can, recommends what to measure based on the endpoints available, picks the right chart for each metric, and builds a self-contained HTML dashboard. Optionally themes it with the user's brand color and logo.
---

# API Dashboard Builder

Build a dashboard from the APIs a team already has, without dragging them through a
20-question intake. The job is to look at what their endpoints can actually return,
recommend a short list of metrics worth watching, pick the right chart for each, and
ship a single self-contained HTML file that pulls the numbers live.

Bias toward building. Ask only what you cannot infer from the endpoints and one line
about the decision the dashboard supports. Never invent a metric the data can't back.

## The flow

### 1. Get the endpoints
Ask the user to share one of, in order of preference:
- an OpenAPI / Swagger spec (URL or file),
- API docs (URL), or
- a list of endpoints, each with a sample JSON response.

If they only give endpoint names, ask for one sample response per endpoint you plan to
use. You need the response shape to write the mapping, so this is the one thing you
cannot skip. Read `references/intake.md` for exactly what to collect and what to infer.

### 2. Propose what to measure (grounded in the endpoints)
From the response shapes, list the metrics the data can actually support, then
recommend 5-9 of them. Favor a mix of **lagging** outcomes (revenue, active accounts,
churn) and **leading** indicators (signups, pipeline, usage) so the dashboard predicts,
not just reports. Prioritize by the decision the user named. `references/metric-frameworks.md`
has the RICE shortlist, the leading/lagging split, and the 5-9 rule.

Then ask **one** focused question and **at most one** follow-up:
- Q1: "What decision does this dashboard support, and who looks at it?" (One line.)
- Optional follow-up, only if the answer changes the tile set: cadence (real-time vs
  weekly) or a must-have metric you didn't propose.

Do not ask more than this. If the user is vague, pick the obvious default, state it, and
build.

### 3. Pick the chart for each metric
Map every chosen metric to a chart type using `references/chart-selection.md`:
- one current number -> `kpi`
- compare / rank categories -> `bar`
- change over time -> `line`
- parts of a whole, 5 slices or fewer -> `donut`

When a metric fits more than one, prefer the simpler read. Put the single most important
number top-left as a large `kpi` tile (people scan dashboards in an F-pattern).

### 4. Offer the brand step (optional)
Ask once, and only once: "Want it in your brand color and logo? Send a hex color and a
logo URL (or file) and I'll theme it. Otherwise I'll use a clean default." Don't block
on this; a `#4F46E5` default looks fine.

### 5. Build it
Copy `references/dashboard-template.html` and fill in two blocks only:
- `THEME`: title, brand hex, optional logo (URL or data: URI).
- `TILES`: one object per metric. For each tile set `type`, `size`, `title`, a
  `source` (`{url, headers, method}`) and a `map(raw)` function that turns the endpoint's
  JSON into the shape the tile needs (documented at the top of the template).

Everything else in the template is the engine; leave it alone. It draws every chart with
inline SVG, so there is nothing to install and nothing loaded from a CDN.

Handle the real-world details:
- **Auth**: put tokens in `source.headers` (e.g. `{"Authorization":"Bearer ..."}`).
  Warn the user that a key in a static file is visible to anyone who opens it, and that
  for anything sensitive they should proxy through a tiny backend or use a read-only key.
- **CORS**: browser `fetch` to a third-party API often fails on CORS. If so, tell them to
  either enable CORS on their side, run behind a proxy, or (for a static snapshot)
  paste sample responses into each tile's `data:` field instead of `source:`.
- **Freshness**: the template already stamps "Updated ..." at load. Keep it.

### 6. Test before you hand it over
Open the file and confirm every tile loads real numbers, no tile is stuck on its
skeleton, and nothing errors. If an endpoint can't be reached from the browser (CORS,
auth), fall back to `data:` with the sample response so the dashboard still renders, and
tell the user which tiles are live vs. sample. A dashboard that half-loads is worse than
one honest snapshot.

## Rules
- 5-9 tiles. If you want more, you're building two dashboards; say so.
- Never show a metric the endpoints can't produce. No invented numbers. If a number needs
  a source you don't have, leave the tile out and note it.
- One big number top-left. Leading and lagging both represented.
- Keep the questions to Q1 plus at most one follow-up. Building beats interviewing.

See `references/example-run.md` for a full worked example, and `examples/example-dashboard.html`
for a rendered dashboard built from this template.
