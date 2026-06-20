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

---

## The Three Belief Buckets

All beliefs live under one of three parent buckets. A user or agent picks a bucket first. The system identifies the right sub-lens within it based on the focus.

| Bucket | What It Watches | Sub-Lenses |
|--------|----------------|------------|
| **Business Memory** | What is durably true about how this business operates as a system | 01–06 |
| **BI Signals** | What is moving, anomalous, or recurring right now | 07–12 |
| **Narrative / Storyline** | How the story is told and where it diverges from the numbers | 13–18 |

---

## What Gets Transformed

Belief sits between raw documents and your AI agents. It transforms three things that no other layer does:

**1. Context-blind agents → calibrated reasoners**
Every agent today starts sessions from zero. With Belief beliefs in context, agents know Q3 always has a one-time EBITDA distortion, that a cost center runs 15% above plan historically. Queries become calibrated to the business, not just syntactically correct.

**2. Format evals → belief-grounded evals**
Current evals check SQL accuracy and output format. Belief enables a new kind of eval: does this answer reflect what is actually true about this business? You can check agent outputs against the world model.

**3. Ephemeral understanding → portable, compounding intelligence**
When you upgrade a model or rebuild a system, the understanding currently lives nowhere — it re-engineers from scratch. With Belief, the world model is portable. A new model inherits the business understanding immediately. The investment compounds instead of resetting.

---

## Repo Structure

```
belief/
├── README.md                           ← you are here
├── BELIEF.md                           ← full specification
│
├── belief-template-system-prompts/     ← the 18 belief reasoning agent prompts
│   ├── README.md
│   ├── 01-business-memory/             ← lenses 01–06
│   ├── 02-bi-signals/                  ← lenses 07–12
│   └── 03-narrative-storyline/         ← lenses 13–18
│
├── architecture/
│   └── overview.md                     ← system architecture & agent integration
│
├── lifecycle/
│   └── ingestion-pipeline.md           ← how documents flow through Belief
│
├── world-model/
│   └── schema.md                       ← belief file structure & update arithmetic
│
├── prompts/
│   └── four-prompt-architecture.md     ← the four core Belief prompts explained
│
└── config/
    └── belief_config.yaml              ← reference configuration file
```

---

## The Belief Lifecycle

Every belief carries:
- **Statement** — a durable, falsifiable interpretation
- **Confidence** — 0.0 to 1.0 (new prior: 0.20, cap: 0.95, floor: 0.05)
- **Evidence** — count of documents that have confirmed it
- **Direction** — Improving / Stable / Deteriorating / Unclear
- **Provenance** — traces to the document and unit that created it

| Update State | Trigger | Effect |
|---|---|---|
| Confirmed | New document supports the belief | Confidence +0.08, Evidence +1 |
| Contradicted | New document challenges the belief | Confidence −0.15, contradiction note appended |
| New Prior | No existing belief covers this signal | Added at confidence 0.20, evidence 1 |
| Decayed | Not seen in 90+ days | Confidence −0.05 per cycle, archived below 0.10 |

---

## The Acceptance Criterion

Give Belief 10 months of business review decks. Read the resulting world model. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — **the system is working**.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## Quick Links

- [Full Specification →](BELIEF.md)
- [18 Belief Reasoning Prompts →](belief-template-system-prompts/README.md)
- [Architecture & Agent Integration →](architecture/overview.md)
- [Ingestion Pipeline →](lifecycle/ingestion-pipeline.md)
- [World Model Schema →](world-model/schema.md)
- [Four-Prompt Architecture →](prompts/four-prompt-architecture.md)
- [Config Reference →](config/belief_config.yaml)
