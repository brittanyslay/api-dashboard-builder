# Intake: collect little, infer a lot

The failure mode of dashboard tools is a long interrogation. Do the opposite. Read the
endpoints, infer the metrics, and confirm with one question.

## What you must collect
1. **The endpoints, with shapes.** An OpenAPI/Swagger spec is ideal because it gives you
   every path and response schema at once. Failing that, API docs, or a list of endpoints
   each with **one real sample JSON response**. You cannot write the `map()` for a tile
   without seeing the response shape, so this is the one non-negotiable.
2. **Auth, if any.** How a request is authorized (bearer token, header key, query param).
   You'll put this in `source.headers`. Flag the security caveat (below).

## Walking an OpenAPI / Swagger spec to a tile plan
A spec hands you every path and response schema at once, so you can skip the sample-paste
step. Walk it like this:
1. **List the GET paths** that return metric-shaped data. Skip auth, webhooks, and write
   endpoints. A `/metrics`, `/billing`, `/accounts`, `/usage`, or `/reports` group is where
   the numbers usually live.
2. **Read each response schema** under `components.schemas` (or the inline `responses`).
   A scalar field like `mrr` or `count` is a `kpi`. An array of objects with a label field
   and a number field is a `bar` or `donut`. An array bucketed by date/week/month is a `line`.
3. **Note auth once** from `securitySchemes` (bearer, API key header, query param), and
   carry it into every tile's `source.headers`.
4. **Take the `example` values** the spec provides as your `map()` test data, so you can
   render and verify before the real key is ever used.
Then propose the 5-9 tiles and confirm, exactly as below. You still ask Q1; the spec just
means you never have to ask for shapes.

## What to infer instead of asking
- **Candidate metrics**: derive them from the fields present. `/subscriptions` with a
  `mrr` field implies MRR; a `status` field implies active/churned counts; a `created_at`
  implies a signups-over-time line.
- **Chart type**: from the metric's shape (see chart-selection.md). A single number is a
  KPI; an array of `{label, value}` is a bar; a time series is a line.
- **Sensible defaults**: currency in USD unless the data says otherwise, weekly buckets
  for trends, "vs last period" deltas where a prior value exists.

## The one question (and at most one follow-up)
- **Q1 (always):** "What decision does this dashboard support, and who looks at it?"
  This tells you which metrics to feature top-left and whether it's exec or ops.
- **Follow-up (only if it changes the tiles):** cadence (live vs. weekly snapshot) or a
  must-have metric you didn't propose from the endpoints.

If the user doesn't answer, choose the obvious default, state your assumption in one line,
and build. You can always adjust a tile after they see it.

## Then confirm the plan in one message
Before building, show the tile plan as a short list so they can veto:

```
Here's the plan (6 tiles):
1. MRR (kpi, top-left)            <- /billing/mrr
2. Active accounts (kpi)          <- /accounts?status=active (count)
3. Net revenue churn (kpi)        <- /billing/churn
4. New signups, 8 weeks (line)    <- /accounts?created_after=... (bucketed)
5. Revenue by plan (bar)          <- /billing/mrr_by_plan
6. Signups by source (donut)      <- /accounts/sources
Brand it in your colors? Send a hex + logo, or I'll use a clean default.
```

One veto round, then build. Don't loop.

## Security caveat to always state
A key placed in a static HTML file is visible to anyone who opens the file or views
source. For anything beyond a personal/local dashboard, tell the user to use a read-only
key, put the key behind a tiny proxy/backend, or ship a static snapshot (paste responses
into each tile's `data:` field) instead of live `source:` calls.
