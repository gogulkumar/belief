# Belief — Business Belief Intelligence

> How a system reads business process deliverables and builds the living model of a business that only a senior analyst accumulates — across any document, any entity, any dimension.

*Gogul Kumar Mathi · 2026*

---

## The Problem

Every organization produces recurring documents as part of how it operates: quarterly business reviews, earnings calls, investor presentations, board decks, operating reports. These are not incidental outputs — they are the formal record of how management interprets performance, explains outcomes, and communicates direction.

What those documents contain, beyond the numbers:

- How this team explains performance — what causes they name, in what order, framed how
- Which metrics they treat as primary versus secondary
- How emphasis shifts when results disappoint
- What language recurs, and what language disappears
- How commitments are explained when they are not met

These behavioral patterns are not visible in any single document. They only emerge after reading many comparable documents from the same entity over time.

A senior analyst who has read ten months of the same business's review decks holds a mental model that no one has written down. They know what a normal deck looks like — so they immediately see what is unusual about this one. They know how this management team typically explains a shortfall. They know which commitment is always ambitious and which is typically conservative.

That accumulated understanding is not in a file. It is not queryable. When the analyst leaves, it leaves with them.

---

## What Belief Is

Belief reads business process deliverables — the recurring decks, transcripts, reports, and documents that organizations already produce — and builds a living model of the behavioral patterns inside them.

Not what any single document says. The pattern of behavior visible only across many comparable documents over time.

It does not store facts. It accumulates **beliefs**.

| A Fact | A Belief |
|--------|----------|
| Revenue was $2.3B in Q3. | Revenue growth is increasingly dependent on pricing rather than volume, suggesting the business is extracting from its existing base rather than expanding it. |
| Static. Answers lookup queries. | Carries direction, confidence, and trajectory. Tells you what to expect in the next document — and what would be a surprise. |

A belief is both **durable** and **provisional**. Durable: it holds across time, not abandoned after one contradicting document. Provisional: it is a working interpretation open to revision when accumulated evidence warrants it. When enough evidence accumulates to demand a different interpretive frame — not just refined confidence, but a different understanding of what the pattern means — the belief shifts perspective, not just statement.

A **skill** does the task — pulls the numbers, builds the report. A **belief** is the understanding the task is performed from. Skills take the doing; beliefs take the thinking.

---

## The Five Belief Types

Every document is read through five belief types. Each asks a different question about the same entity. All five write into one world model — one living model of one entity.

| # | Belief Type | What It Asks |
|---|-------------|-------------|
| 01 | Business Memory | What is structurally and durably true about how this entity is built? |
| 02 | Business Dynamics | How does the entity behave as a system — where does pressure build, where does it release? |
| 03 | Narrative Understanding | How does the organization tell its story, and what does the pattern of language reveal? |
| 04 | Factual Understanding | What has been quantitatively observed, and which observations are building toward a belief? |
| 05 | Causal Understanding | Which signals reliably predict which outcomes in this entity, and with what lag? |

Custom belief streams are also supported: Marketing Efficiency Memory, Forecast Reliability, Operational Risk Memory, Investor Messaging, Margin Quality, Forecast Bias, or any user-defined angle.

---

## How Belief Works

0. **Entity foundation built** — one interview session builds `foundation.md`: the business model, thesis metrics, normalization model, and narration design for the entity
1. **Stream setup** — defines the entity, the angle, and the document set
2. **Blueprint compiled** — defines what a belief looks like for this entity in this angle, grounded in the foundation; seeds 8–15 candidate belief hypotheses
3. **Prompts compiled** — one belief reasoning prompt and one fact extraction prompt, carrying the foundation context
4. **Document arrives** — any format: deck, transcript, report, audio
5. **Signals extracted** — the fact extractor captures raw, granular signals from the document at the level needed for 8–15 distinct Candidate beliefs
6. **Belief evolves** — the belief engine reads the signals and makes surgical updates to the numbered belief list
7. **Silence by default** — most inputs produce no update. The gate is the mechanism.

Each belief carries:
- **Claim as heading** — `## Belief #N — [specific, falsifiable sentence]`
- **Statement** — the claim restated precisely as business judgment grounded in the foundation
- **Why it matters** — how it connects to the entity's profitability thesis
- **Evolution trail** — first-person, per-document journey of how the pattern developed
- **Normal baseline** — what the next comparable document should show if holding
- **Falsification test** — what a future document must show to break, narrow, or retire it

---

## Repo Structure

```
belief/
├── README.md                               ← you are here
├── BELIEF.md                               ← full specification
├── usage.md                                ← how beliefs accumulate and how to use them
│
├── architecture/
│   └── overview.md                         ← system architecture & how it fits in the stack
│
├── lifecycle/
│   └── ingestion-pipeline.md               ← how documents flow through Belief (foundation + two phases)
│
├── world-model/
│   └── schema.md                           ← belief file structure, update arithmetic, changelog tags
│
├── prompts/
│   ├── belief-doctrine.md                  ← shared doctrine: what a belief is across all prompts
│   ├── -1-generate-foundation.md           ← Prompt -1: build the entity foundation (once per entity)
│   ├── 00-document-profile.md             ← Prompt 00: interview agent to build document profile
│   ├── 01-generate-blueprint.md           ← Prompt 01: produce the Strategic Blueprint
│   ├── 03-belief-reasoning-compiler.md    ← Prompt 03: compile the belief reasoning prompt
│   └── 06-fact-extractor.md              ← Prompt 06: compile the fact extraction prompt
│
├── config/
│   └── belief_config.yaml                  ← reference configuration file
│
├── entities/                               ← one subdirectory per entity; built before any stream
│   └── {entity_id}/
│       └── foundation.md                   ← business model, thesis metrics, normalization, narration
│
├── compiled/                               ← generated at setup; one subdirectory per stream
│   └── {stream_id}/
│       ├── document_profile.md
│       ├── strategic_blueprint.md
│       ├── belief_reasoning_prompt.md
│       └── fact_extractor_prompt.md
│
└── streams/                                ← living data; one subdirectory per stream
    └── {stream_id}/
        ├── belief.md                       ← L1 world model (surgically updated, numbered beliefs)
        ├── belief_changelog.md             ← append-only audit trail
        ├── L2_factlogs/                    ← per-document extracted signals
        └── L3_raw/                         ← immutable transcription archive
```

---

## The Acceptance Criterion

Give Belief ten months of business review decks. Read the resulting world model. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — **the system is working**.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## Quick Links

- [Full Specification →](BELIEF.md)
- [How Belief Accumulates and How You Use It →](usage.md)
- [Shared Belief Doctrine →](prompts/belief-doctrine.md)
- [Architecture & Stack Integration →](architecture/overview.md)
- [Ingestion Pipeline →](lifecycle/ingestion-pipeline.md)
- [World Model Schema →](world-model/schema.md)
- [Config Reference →](config/belief_config.yaml)
