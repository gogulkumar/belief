# Belief — Business Belief Intelligence

> Institutional memory is portable: what is normal, what is template, what is evolving, and what would break — loaded into agents at the moment they draft, validate, or flag — instead of re-teaching the model from raw decks every time.

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

Each belief carries four kinds of institutional understanding, each mapped to a field in the belief entry:

| What a belief captures | Belief field | What it gives you |
|---|---|---|
| **What is normal** | Normal baseline | Expected ranges and behaviors — deviations are immediately visible |
| **What is template** | Foundation signals-vs-noise | What this entity always says, what language is formulaic, what to discount so the real signal stands out |
| **What is evolving** | Evolution trail | Per-document history of how the pattern developed — deepened, tensioned, narrowed |
| **What would break** | Falsification test | The specific signal in a future document that would contradict, narrow, or retire the belief |

These four things — loaded into an agent at the start of a drafting, validation, or flagging task — replace the need to re-teach the model from raw decks every time.

A **skill** does the task — pulls the numbers, builds the report. A **belief** is the understanding the task is performed from. Skills take the doing; beliefs take the thinking.

---

## The Five Belief Types

Every document is read through five belief types. Each asks a different question about the same entity. All five write into one belief memory — one living model of one entity.

| # | Belief Type | What It Asks |
|---|-------------|-------------|
| 01 | Learning How Finance Consistently Explains Itself | What behavioral and communication patterns has this team established across recurring documents? |
| 02 | Learning How This Business Generates and Loses Value | How does the entity behave as a system — where does pressure build, where does it release? |
| 03 | Learning How Documents Frame the Story | How does the organization tell its story, and what does the pattern of language reveal? |
| 04 | Learning What Finance Has Recorded as True | What has been quantitatively observed, and which observations are building toward a belief? |
| 05 | Learning What Kind of Business This Actually Is | What is the durable structural truth about what this business is, how it makes money, and why it performs the way it does? |

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
├── belief-memory/
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
        ├── belief.md                       ← L1 belief memory (surgically updated, numbered beliefs)
        ├── belief_changelog.md             ← append-only audit trail
        ├── L2_factlogs/                    ← per-document extracted signals
        └── L3_raw/                         ← immutable transcription archive
```

---

## The Acceptance Criterion

Give Belief ten months of business review decks. Read the resulting belief memory. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — **the system is working**.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## Quick Links

- [Full Specification →](BELIEF.md)
- [How Belief Accumulates and How You Use It →](usage.md)
- [Shared Belief Doctrine →](prompts/belief-doctrine.md)
- [Architecture & Stack Integration →](architecture/overview.md)
- [Ingestion Pipeline →](lifecycle/ingestion-pipeline.md)
- [Belief Memory Schema →](belief-memory/schema.md)
- [Config Reference →](config/belief_config.yaml)
