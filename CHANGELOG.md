# Changelog

## v1.1.0 - 2026-11-05

Best-in-class overhaul of the output and the workflow.

- **Charts you can read exact numbers off of.** Bars now label each value, line charts show
  the latest value in the caption, the donut keeps its share labels, and every mark has a
  hover tooltip.
- **"Lower is better" done right.** Tiles can set `goodWhenDown:true` so a drop in churn,
  cost, latency, or cycle time shows green instead of red.
- **Sturdier live fetch.** Each tile fetch times out instead of hanging, a failed tile shows
  an error with a Retry button rather than a dead card, and an optional `refresh` interval
  keeps the dashboard live and restamps the time.
- **A real self-critique step.** SKILL.md now has the skill render its own dashboard and
  check each chart fits its data, both themes read, and no number is invented, before handing
  anything back.
- **OpenAPI straight to a tile plan.** Intake docs now walk a spec's paths and schemas to a
  plan without a sample-paste round trip.
- Re-linked the README hero image, regenerated from the repo's own
  `examples/example-dashboard.html` so it stays real output, not a mockup.

## v1.0.1 - 2026-08-20

Pulled the README preview image for now while all the public repos' preview images get reconsidered together. The image file is still in the repo, just not linked from the README.


## v1.0.0 - 2026-08-20

Initial release: reads your API endpoints (or an OpenAPI spec), proposes 5-9 metrics worth watching, picks the right chart per metric, and ships a single self-contained live HTML dashboard. Preview images verified against the repo's own `examples/example-dashboard.html` - real output, not a mockup.
