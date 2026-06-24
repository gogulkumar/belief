# Seeding — Building the Belief Base Before Documents Arrive

The ingestion pipeline begins when a document arrives. But the belief system works better if it already has a foundation before the first document is processed. That foundation is the Knowledge Dossier.

---

## The Problem It Solves

A belief extracted from the first document has no prior context. The system cannot distinguish signal from noise. It cannot tell if a metric is anomalous because it has no baseline. Seeding gives it that baseline before document ingestion begins.

---

## The Knowledge Dossier

The Knowledge Dossier is a structured, human-written document that captures the institutional understanding of one business — the understanding a senior analyst carries in their head after months in the room.

It is not a belief. It does not carry confidence scores or direction. It is the static textbook the belief system reads from on day one, before any document has arrived.

### The Three Pillars

**Revenue Engine** — How does this business actually make money?
- Business model mechanics (e.g., merchant vs. agency model, take rate drivers)
- Loyalty program economics and how they affect contra-revenue
- Key revenue levers and their known sensitivities

**Cost Drivers** — Where does the money go?
- Customer acquisition costs by channel and how they vary with demand
- Fixed vs. variable cost behavior at different volume levels
- Known cost centers and their normal operating ranges

**Core Constraints** — What are the fixed limits the business operates within?
- Seasonality patterns and how they manifest in the numbers
- Competitive dynamics that cap pricing power
- Structural or regulatory limits on growth

### Format

The dossier is prose, not structured data. It is written by a domain expert or extracted from existing documentation — prior briefing documents, onboarding materials, planning decks. It does not follow the belief entry format and is never processed through the standard ingestion pipeline.

---

## Metric-Dynamic Anchor Table

Before documents arrive, define the core KPIs and the belief angle for each. This table maps a metric movement to a candidate business dynamic — so the system knows what to look for when it sees a number move.

| Metric | Movement | Candidate Business Dynamic |
|--------|----------|---------------------------|
| Take Rate | Drops 40+ bps | Loyalty redemption dilution or channel mix shift |
| Room Nights | Declines while revenue holds | Price-led extraction from existing base, not volume expansion |
| Marketing Spend / GBV | Rising above historical band | Demand softness requiring paid channel support |
| EBITDA Margin | Compresses despite revenue growth | Fixed cost leverage failing; volume not scaling the cost base |
| App Churn Rate | Rising | Loyalty program not delivering retention value |

The anchor table seeds the Causal Understanding belief type. When the system sees Take Rate drop in a document, it has a candidate interpretation to confirm or challenge — rather than starting from zero.

---

## The Three-Level Cascade

Beliefs operate at three levels of scope. The Knowledge Dossier populates each level differently, and the evidence threshold for revising a belief scales with its level.

```
┌─────────────────────────────────────────────────────────────┐
│ LEVEL 1 — GLOBAL FINANCE BELIEFS                            │
│ Universal accounting rules. Apply to any document.          │
│ "P&L lines must reconcile. Revenue = Volume × Take Rate."   │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ LEVEL 2 — BRAND SCOPE BELIEFS                               │
│ Business model rules specific to this brand.                │
│ "HCOM is a loyalty-led retail engine. Monitor app churn."   │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ LEVEL 3 — DOMAIN METRIC BELIEFS                             │
│ Tolerance thresholds for specific KPIs.                     │
│ "Marketing/GBV above X% signals demand softness."           │
└─────────────────────────────────────────────────────────────┘
```

**Level 1** beliefs are near-permanent. They change only when accounting standards or fundamental business model mechanics change. A single document that appears to contradict a Level 1 belief is more likely an extraction error than a genuine contradiction.

**Level 2** beliefs drift slowly — over quarters, not months. They update when the brand's strategic direction or business model shifts.

**Level 3** beliefs are the most volatile. Domain thresholds shift with each business cycle and should be revisited on every ingestion run.

---

## How the Dossier Enters the World Model

The Knowledge Dossier does not go through the standard ingestion pipeline (Prompt 2 → Prompt 3). Instead:

1. A human or setup agent reads the dossier and extracts candidate beliefs at each scope level.
2. Each candidate is written directly into the world model as a **seeded prior** — confidence 0.20, direction Unclear.
3. The source for each seeded belief is recorded as `[Knowledge Dossier — {date}]`.
4. From this point, the standard ingestion pipeline takes over. Documents confirm, contradict, or decay the seeded beliefs exactly as they would any other belief.

The dossier itself is preserved in L3 (Raw Archive) for reference. It is never reprocessed.

---

## What Seeding Enables

Without seeding, the first document the system reads is interpreted in a vacuum.

With seeding:
- The system can immediately distinguish a metric that is anomalous from one that is within normal range.
- The fact-to-belief gate has baseline context to evaluate new signals against.
- Causal beliefs seed at 0.20 from the anchor table and accumulate evidence forward — rather than waiting for a second occurrence before any belief exists.
- The world model does not start empty. It starts with the working understanding a new analyst would take months to absorb.
