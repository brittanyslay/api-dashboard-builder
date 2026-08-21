# Worked example

How a real run goes, start to finish. This is the dashboard in `examples/example-dashboard.html`
and the screenshot in the README.

## 1. User brings endpoints
> "Build me a dashboard for our SaaS. We have a billing API and an accounts API."

Claude asks for shapes, user pastes samples:

```
GET /billing/mrr            -> { "current": 128400, "change_pct": 6.2 }
GET /billing/churn          -> { "rate": -1.4 }
GET /billing/mrr_by_plan    -> { "plans": [ {"name":"Starter","mrr":18200}, ... ] }
GET /accounts/summary       -> { "count": 2140, "change_pct": 3.1 }
GET /accounts/signups_weekly-> { "weekly": [ {"w":"W1","n":82}, ... ] }
GET /accounts/sources       -> { "src": [ {"k":"Organic","v":540}, ... ] }
```

## 2. Claude proposes the plan (one message)
> Here's the plan (6 tiles), a mix of outcomes and leading indicators:
> 1. **MRR** (kpi, top-left): the number the business runs on
> 2. **Active accounts** (kpi)
> 3. **Net revenue churn** (kpi, lower is better)
> 4. **New signups, 8 weeks** (line): the leading indicator for #1
> 5. **Revenue by plan** (bar)
> 6. **Signups by source** (donut)
>
> Want it in your brand colors? Send a hex + logo, or I'll use a clean default.

User: *"Use our gradient: #8B5CF6, #EC4899, #F59E0B."*

## 3. Claude builds it
Copies `references/dashboard-template.html`, sets `THEME` and one tile per metric. Two
representative tiles:

```js
const THEME = { title:"Northwind SaaS: Growth", brand:"#8B5CF6",
                gradient:["#8B5CF6","#EC4899","#F59E0B"] };

const TILES = [
  { id:"mrr", type:"kpi", size:"lg", title:"Monthly Recurring Revenue",
    source:{ url:"/billing/mrr" },
    map:(r)=>({ value:r.current, delta:r.change_pct, deltaLabel:"vs last month", format:"usd" }) },

  { id:"signups", type:"line", size:"md", title:"New signups", subtitle:"last 8 weeks",
    source:{ url:"/accounts/signups_weekly" },
    map:(r)=>({ points:r.weekly.map(p=>({ x:p.w, y:p.n })) }) },
  // ... four more
];
```

Note the pattern: `source.url` is the endpoint, and `map(raw)` reshapes that endpoint's
JSON into the tile's shape. Nothing else in the template changes.

## 4. Claude tests it before handing it over
Opens the file, confirms all six tiles load real numbers, none stuck on a skeleton, no
console errors, gradient renders. If `/billing/mrr` had failed CORS from the browser,
Claude would swap that tile to `data:` with the sample response and tell the user tiles
1-3 are live, 4-6 are a snapshot, here's how to make them all live.

## 5. Handoff
One HTML file. The user opens it, or drops it on any static host. It refetches every time
it loads and stamps the time at the top.
