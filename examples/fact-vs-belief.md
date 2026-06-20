# Fact vs Belief — The Same Observation, Two Treatments

The most common error in a belief system is promoting an observation to
a belief before it has earned that status. This file shows one raw
observation handled correctly (filed as a fact) and incorrectly (held
as a belief), with explanation of why the distinction matters.

---

## The Raw Observation

From a quarterly review:

> "Customer acquisition cost for the period was $142, up from $118
> in the prior period — an increase of approximately 20%. The increase
> reflects higher media spend and increased competition for digital
> inventory in Q3."

---

## Treatment A — Correct: Filed As A Fact

**Where it goes:** L3 Archive + Factual Understanding signal tracker

**Archive entry:**
Metric: Customer acquisition cost
Period: Q3 Cycle 7
Value: $142 (prior: $118, change: +$24, +20%)
Attribution in document: Higher media spend, Q3 digital inventory competition

**Signal tracker note (FU-T05):**
Acquisition cost rose 20% in one period. Management attribute to seasonal
media market. Single observation. Prior periods: Cycle 5 at $112, Cycle 6
at $118 — the cost was already rising modestly before this jump. Watching
Cycle 8 for whether this is seasonal reversal or a sustained increase.

**Why this is correct:**
A 20% increase in one quarter is notable. But notable is not durable.
The management attribution (Q3 media market) is plausible and testable —
if Q4 shows acquisition cost returning toward $118–$120, the attribution
holds. One period does not establish a pattern. Filing it keeps it visible
without locking in an interpretation that the evidence does not yet support.

**What happens next:**
If acquisition cost rises again in Cycle 8 without the Q3 seasonal
explanation, the signal has now appeared twice in a non-seasonal context.
The question then becomes: what does this recurring increase imply about
the business's growth economics? That answer — if falsifiable and
distinctive — becomes a belief in Business Memory or Business Dynamics.

---

## Treatment B — Incorrect: Held As A Belief

**Where it goes (incorrectly):** Business Memory, seeded as BM-06

**Belief statement:**
"Customer acquisition cost is rising, suggesting the business's primary
growth channel is approaching diminishing returns and new customer
economics are deteriorating."
Confidence: 0.20 | Direction: Deteriorating | Evidence: 1

**Why this is wrong:**

**1. One data point is not a pattern.**
A belief in the Business Memory lens requires something durable — a
structural truth that would survive a bad quarter. A single quarter's
acquisition cost is the opposite of durable. It may reverse immediately.

**2. The management explanation has not been tested.**
The belief asserts "approaching diminishing returns" — a structural claim
about the channel economics. The evidence supports only "costs rose in
Q3, attributed to media market competition." These are not the same thing.
The belief is asserting a structural interpretation that the evidence does
not yet warrant.

**3. It fails the falsifiability test as written.**
"Is rising" will be falsified by any quarter where acquisition cost
declines. A belief that changes with every new data point is not a
belief — it is a tracked metric wearing a belief label.

A properly falsifiable version would need: "Acquisition cost has risen
for five consecutive periods despite stable media conditions, suggesting
the core growth channel is losing efficiency as the addressable customer
pool saturates." That requires five periods, a comparison to market
conditions, and a mechanism. This observation does not have any of those.

**4. The harm of holding this incorrectly:**
The next document shows acquisition cost declining to $125 in Q4. The
system treats this as contradicting BM-06: confidence drops from 0.20
to 0.05 (below floor, removed). The world model now has a record of a
belief being "contradicted" — but the belief was never valid. The system
has processed noise as signal and the confidence history for this belief
is corrupted.

More importantly: if a real acquisition cost deterioration develops over
the next eight quarters, the system will have less clean signal to work
with because an early false positive was filed, updated, and removed,
cluttering the evidence trail.

---

## The Rule

**A fact describes what happened.** Always accepted. Dated, filed,
immutable in the archive. Does not require falsifiability.

**A belief describes what it means — durably.** Requires recurrence,
a falsifiable interpretation, and specificity to this business. Earned
over multiple observations.

**The test before seeding:**
Can I write this as a statement that a single future data point would
NOT immediately contradict? If no — it is a tracked signal, not a belief.

| | Fact | Belief |
|--|------|--------|
| Requires recurrence | No | Yes |
| Requires falsifiability | No | Yes |
| Changes with each data point | By design | Never |
| Lives in | L3 Archive + FU tracker | World model (L1) |
| Valid from | First observation | Pattern of observations |
