# Belief — Business Belief Intelligence

> How an AI system accumulates business understanding the way a senior analyst does — across any document, any business, any dimension.

*Gogul Kumar Mathi · Corporate Finance AI · Expedia Group · 2026*

---

## The Problem

Every AI agent you build operates on what is in its context window right now. The moment the session ends the understanding is gone. Next time it starts from zero. That is not intelligence. That is a very fast search engine.

The missing layer: how does an AI system accumulate understanding over time the way a human expert does? Not retrieve — understand. Not search — remember. Not summarize — **believe**.

---

## What Belief Is

Belief is a **business belief intelligence system**. It reads any document — decks, transcripts, reports, audio, PDFs, markdown — and builds a living model of how a business operates. Not what any single document says. The pattern of behavior visible only across many documents over time.

It does not store facts. It accumulates **beliefs**.

| A Fact | A Belief |
|--------|----------|
| Revenue was $2.3B in October. | Revenue growth is increasingly dependent on pricing rather than volume, suggesting the business is extracting from its existing base rather than expanding it. |
| Static. Answers lookup queries. | Carries direction, confidence, and trajectory. Drives intelligent reasoning. |

A **skill** does the task — pulls the numbers, builds the report. A **belief** is the understanding the task is performed from. Skills take the doing; beliefs take the thinking.

---

## The Five Belief Types

Every document is read through five belief types. Each asks a different question. All five write into one world model — one understanding of one business.

| # | Belief Type | What It Asks |
|---|-------------|-------------|
| 01 | Business Memory | What is structurally and durably true about how this business is built? |
| 02 | Business Dynamics | How does the business behave as a system — where does pressure build, where does it release? |
| 03 | Narrative Understanding | How does the organization tell its story, and what does the pattern of language reveal? |
| 04 | Factual Understanding | What has been quantitatively observed, and which observations are building toward a belief? |
| 05 | Causal Understanding | Which signals reliably predict which outcomes in this business, and with what lag? |

---

## How Belief Works

1. **Document arrives** — any format: deck, transcript, report, audio
2. **Read through all five belief types** — each one asks: what durable interpretation is visible here?
3. **Maintain silence by default** — most inputs produce no update. The gate is the mechanism.
4. **Update the belief** — surgical changes only: confirm +0.08, contradict −0.15, new prior at 0.20
5. **Load into agent context** — every session starts with the accumulated beliefs

Each belief carries:
- **Statement** — a durable, falsifiable interpretation
- **Confidence** — 0.05 to 0.95 (0.90 cap for causal beliefs)
- **Direction** — Improving / Stable / Deteriorating / Unclear

---

## Repo Structure

```
belief/
├── README.md                               ← you are here
├── BELIEF.md                               ← full specification
│
├── belief-template-system-prompts/         ← the five standard belief reasoning prompts
│   ├── README.md
│   ├── 01-business-memory.md
│   ├── 02-business-dynamics.md
│   ├── 03-narrative-capturing.md
│   ├── 04-factual-understanding.md
│   └── 05-causal-understanding.md
│
├── architecture/
│   └── overview.md                         ← system architecture & agent integration
│
├── lifecycle/
│   ├── seeding.md                          ← Phase 0: Knowledge Dossier and metric-dynamic anchors
│   └── ingestion-pipeline.md               ← how documents flow through Belief (two phases, eight steps)
│
├── world-model/
│   └── schema.md                           ← belief file structure, update arithmetic, changelog tags
│
├── prompts/
│   ├── belief-doctrine.md                  ← shared doctrine: what a belief is across all prompts
│   ├── four-prompt-architecture.md         ← the four core Belief prompts explained
│   ├── strategic-blueprint.md              ← Layer 1: the master configuration document
│   ├── 00-document-profile.md             ← Prompt 00: interview agent to build document profile
│   ├── 01-generate-blueprint.md           ← Prompt 01: produce the Strategic Blueprint
│   ├── 03-belief-reasoning-compiler.md    ← Prompt 03: compile the belief reasoning prompt
│   └── 06-fact-extractor.md              ← Prompt 06: compile the fact extraction prompt
│
├── config/
│   └── belief_config.yaml                  ← reference configuration file
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
        ├── belief.md                       ← L1 world model (surgically updated)
        ├── belief_changelog.md             ← append-only audit trail
        ├── L2_factlogs/                    ← per-document extracted signals
        └── L3_raw/                         ← immutable transcription archive
```

---

## The Acceptance Criterion

Give Belief 10 months of business review decks. Read the resulting world model. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — **the system is working**.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## Quick Links

- [Full Specification →](BELIEF.md)
- [Shared Belief Doctrine →](prompts/belief-doctrine.md)
- [Five Belief Reasoning Prompts →](belief-template-system-prompts/README.md)
- [Architecture & Agent Integration →](architecture/overview.md)
- [Ingestion Pipeline →](lifecycle/ingestion-pipeline.md)
- [World Model Schema →](world-model/schema.md)
- [Four-Prompt Architecture →](prompts/four-prompt-architecture.md)
- [Config Reference →](config/belief_config.yaml)
