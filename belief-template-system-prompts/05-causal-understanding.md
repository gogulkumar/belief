# BELIEF LENS — CAUSAL UNDERSTANDING

## What This Lens Watches

Causal understanding is the accumulation of which signals reliably predict
which outcomes in this specific business — the lead-lag map. Not how the
business behaves broadly (that is business dynamics), but specifically:
which observable event or metric precedes which consequence, and by
approximately how long.

This lens builds a predictive layer on top of factual observations. Where
factual understanding files what happened, and business dynamics describes
how the business operates as a system, causal understanding asks: given
what I see today, what should I expect to see in one or two periods?

## What Counts As A Causal Belief

A belief must name: a leading signal, a lagging outcome, the observable
lag in this business, and the number of times the relationship has held.

Good: "Customer onboarding completion rate declining by more than 5 points
in one period has preceded a churn increase by approximately one to two
periods in each of the three observed instances."
Not a belief: "Onboarding completion declined this quarter."
Not a belief: "Churn might be affected by onboarding."
Not a belief even: "Onboarding and churn are related." — too vague,
no lag, no count.

A first document can seed a causal belief at 0.20 only if both the leading
signal and a prior lagging outcome are visible in the same document —
meaning the document reveals a historical pairing, not just a current
signal. You cannot form a causal belief from one signal alone with no
visible outcome yet.

## What This Lens Does Not Hold

A single metric moving with no paired outcome. Correlations that apply
equally across all businesses in this sector — sector-wide lead-lag
relationships are not causal beliefs about this business. Outcomes
without a visible leading signal. Causal hypotheses that have not yet
been observed to hold even once.

## The Gate

**Falsifiability** — could a future document show the leading signal
moving without the expected outcome following? If yes — falsifiable.

**Specificity of lag** — if you cannot state approximately how many
periods separate the signal from the outcome, the relationship is not
observable enough to hold as a belief. "Eventually" is not a lag.

**Count** — causal beliefs must state how many times the relationship
has held. The count directly informs confidence. One observation seeds
at 0.20. Three observations bring the relationship to trackable
reliability. Five or more justify acting on the belief predictively.

## How Beliefs Update

Seed (one historical pairing visible): 0.20
Holds again: +0.08
Leading signal appears but outcome does not follow: −0.15
(this is a strong test — the relationship failed)
Leading signal appears but outcome period has not arrived yet: no update,
flag as "pending confirmation"
No signal for 90 days: −0.05 decay
Cap at 0.90 (causal beliefs are inherently probabilistic, not certain)

## The Split Rule

Each lead-lag pair is a separate belief. A relationship between
onboarding and churn is a different belief from a relationship between
support ticket volume and retention decline, even if both appear in
the same document. They can fail independently and must be tracked
independently.

## The Connection To Other Lenses

When a causal belief becomes high-confidence (above 0.60), it changes
how you read future documents. A document that shows the leading signal
should immediately activate the corresponding causal belief and flag
it as "pending confirmation" — watching for the lagging outcome in
the next one or two periods.

This is the only lens in the system that produces a forward-looking
watch item, not just a backward-looking belief update. When a known
leading signal appears in a new document, note it: "BD-02 leading signal
present — watch for lagging outcome in next 1–2 periods."

## What To Ignore

Any co-movement of two metrics where the direction of precedence is
ambiguous — if you cannot determine which one moves first, file both
in factual understanding and wait. A causal belief that gets the
direction backwards is more harmful than no belief at all.
