# What to measure

The point of the intake is to land on 5-9 metrics that actually drive a decision, using
only what the endpoints can produce. These are the frameworks to reason with.

## The 5-9 rule
A screen holds 5 to 9 metrics before it turns into noise. If the user wants more, you're
building two dashboards (e.g. an exec view and an ops view). Say so and split them.

## Leading vs. lagging (include both)
- **Lagging** metrics report outcomes: revenue, active accounts, churn, closed deals.
  They tell you where you ended up.
- **Leading** metrics predict them: signups, trials, pipeline created, usage, reply rates.
  They tell you where you're heading.

A dashboard of only lagging metrics is a rear-view mirror. Always pair at least one
leading indicator with the outcome it predicts.

## Prioritize with RICE (lightweight)
When more metrics are available than fit, rank candidates by Reach (how many decisions it
touches), Impact (how much it moves the decision), Confidence (how trustworthy the data
is), over Effort (how hard it is to compute from the endpoints). Keep the top 5-9.

## Balanced-scorecard sanity check
For an exec dashboard, make sure the set spans more than one lens: Financial, Customer,
Operational, and Growth/Learning. If every tile is financial, you're measuring the
scoreboard and none of the game.

## Layout follows attention
- People scan dashboards in an **F-pattern**: top-left first, then across, then down the
  left edge. Put the single most important number top-left as a large `kpi`.
- Group related tiles next to each other (all revenue together, all acquisition together).
- Show a **last-updated timestamp** (the template does this automatically). In 2026,
  freshness transparency is table stakes for trust.

## Don't measure what the data can't support
Every proposed metric must map to a real field in a real endpoint response. If a metric
the user wants needs a source you don't have, leave the tile out and tell them what
endpoint would unlock it. An honest 6-tile dashboard beats a 9-tile one with two made-up
numbers.
