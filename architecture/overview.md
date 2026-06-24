# Architecture Overview — Belief in the Stack

## The Three-Layer Architecture

Belief is built in three layers. Each layer has a distinct responsibility and runs at a different time.

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 1 — CONFIGURATION                                      │
│  Strategic Blueprint                                          │
│  Who the entity is · what angle · what watch areas           │
│  What patterns look like · what a belief looks like here      │
│  Runs: once at setup time                                     │
└──────────────────────────┬───────────────────────────────────┘
                           │  (read once, flows into both compilers)
            ┌──────────────┴──────────────────┐
            ▼                                  ▼
┌─────────────────────────┐       ┌──────────────────────────┐
│  LAYER 2 — COMPILATION  │       │  LAYER 2 — COMPILATION   │
│  Belief Reasoning Prompt│       │  Fact Extraction Prompt  │
│  (angle-aware,          │       │  (watch-area-scoped,     │
│   entity-grounded)      │       │   pattern-direction-aware)│
│  Runs: once per stream  │       │  Runs: once per doc type  │
└────────────┬────────────┘       └──────────────┬───────────┘
             │                                    │
             └──────────────┬─────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  LAYER 3 — EXECUTION                                          │
│  Document → L3 units (transcription windows)                  │
│          → L2 fact log (per document, angle-scoped)           │
│          → belief evolution (existing belief + fact log)      │
│          → changelog (what changed, what drifted)             │
│  Runs: every new document                                     │
└──────────────────────────────────────────────────────────────┘
```

### The Cascade Principle

The blueprint (Layer 1) is compiled once at setup time. Every downstream step reads it. If the blueprint is wrong or incomplete, everything below it is wrong. If the blueprint is rich and precise, everything below it is rich and precise — without any changes to the execution layer.

This is the most important architectural property of the system: quality is injected at Layer 1 and propagates forward. Debugging a bad belief output should start by examining the blueprint, not the execution prompt.

### What Changes Cascade

A change to the blueprint automatically propagates to:
- The belief reasoning prompt (Layer 2A) — which reads the blueprint for entity context and angle definition
- The fact extraction prompt (Layer 2B) — which reads the blueprint for watch areas and pattern direction
- The world model (Layer 3) — which accumulates beliefs shaped by the above two

No code changes required. The compilers are generic. Only the blueprint is entity-specific.

---

## Where Belief Sits

Business process deliverables sit at the bottom of the stack. Belief reads those deliverables and builds the pattern model that sits above retrieval and below any agent or analyst that needs to reason about the business.

```
┌─────────────────────────────────────────────────────────┐
│              AGENTS / ANALYSTS                           │
│         Query agents · Report builders · Evaluators     │
└──────────────────────┬──────────────────────────────────┘
                       │  belief context loaded at session start
                       ▼
┌─────────────────────────────────────────────────────────┐
│                   BELIEF LAYER                          │
│         World Model (L1) — living belief memory         │
│         Fact-to-belief gate · Update arithmetic         │
└──────────────────────┬──────────────────────────────────┘
                       │  evidence retrieval
                       ▼
┌─────────────────────────────────────────────────────────┐
│                 FACT LOGS / ARCHIVE                     │
│         L2 Fact Logs (per document) · L3 Raw Archive    │
└──────────────────────┬──────────────────────────────────┘
                       │  reads from
                       ▼
┌─────────────────────────────────────────────────────────┐
│           BUSINESS PROCESS DELIVERABLES                 │
│    Quarterly reviews · Earnings calls · Board decks     │
│    Investor presentations · Operating reports · Audios  │
└─────────────────────────────────────────────────────────┘
```

Belief is not a retrieval system, a chat interface, or a database. It is the **pattern layer** — the accumulated interpretation of how this entity behaves, built from the documents the entity already produces.

## How Agents Use Belief

Agents do not query Belief at runtime. The world model is **loaded into context at session start** — the same way a senior analyst reads their notes before a meeting. Every inference is then implicitly grounded by accumulated business understanding.

### Query Agent
Without Belief: queries data blind — syntactically correct answers, contextually naive.
With Belief: knows Q3 always has a one-time cost distortion. Knows a cost center runs 15% above plan historically. Answers become calibrated, not just correct.

### Narrative / Comms Agent
Without Belief: writes from static voice profiles frozen at setup.
With Belief: knows how management framing has evolved. Knows when the team shifted from growth to efficiency language. Content reflects the actual evolving communication pattern.

### Evaluation
Without Belief: checks output format and data accuracy.
With Belief: checks whether an answer reflects what is actually true about how this business operates. A new category of evaluation that cannot exist without a pattern layer.

---

## The Setup and Execution Flow

### Setup — runs once per belief stream

```
Prompt 00 ← (entity, document types, angle, prior knowledge)
         → document_profile.md

Prompt 01 ← (document_profile.md, stated purpose)
         → strategic_blueprint.md

Prompt 03 ← (strategic_blueprint.md)
         → belief_reasoning_prompt.md

Prompt 06 ← (strategic_blueprint.md, belief_reasoning_prompt.md, doc type)
         → fact_extractor_prompt.md
```

### Execution — runs for every document

```
intake.py          ← uploaded document (any format)
                   → L3_raw/{doc_id}.json (immutable)

fact_extractor.py  ← fact_extractor_prompt.md (system)
                   + L3 units + topics_touched
                   → L2_factlogs/{doc_id}_fact_log.md

belief_engine.py   ← belief_reasoning_prompt.md (system)
                   + existing belief.md (or NULL)
                   + fact log
                   → belief.md (evolved)
                   → belief_changelog.md (appended)
```

The belief engine at runtime receives only its compiled prompt, the existing world model, and the fact log. It never receives the raw document or the blueprint. This is the contract that makes the system reproducible and auditable.

---

## The Three Storage Layers

| Layer | Purpose | Mutability |
|-------|---------|------------|
| **L1 — World Model** | Living belief memory. One `belief.md` per stream. Loaded into agent context. | Surgically updated per document |
| **L2 — Fact Logs** | Per-document extracted signals, organized by watch area. | Written once per document |
| **L3 — Raw Archive** | Original document content verbatim. | Immutable. Written once. |

---

## Pattern Recognition Is the Method, Not a Sixth Type

Pattern recognition is not a separate belief type. It is the method that all five types apply. Every belief type is about patterns — the difference is what form patterns take:

| Belief Type | Pattern Form |
|-------------|-------------|
| Business Memory | Cross-period behavioral recurrence; language the documents use to mark repetition |
| Business Dynamics | Structural ratio stability and trajectory across periods |
| Narrative Understanding | Linguistic fingerprints; sequencing habits; what appears vs what is absent |
| Factual Understanding | Metric definition stability; benchmark comparison consistency |
| Causal Understanding | Lead-lag pair recurrence; signal → outcome with consistent lag |

The fact extraction prompt is the injection point for pattern direction. When the blueprint specifies what form patterns take for a given watch area, the fact extractor captures not just values but recurrence signals — the actual language used, the structural positions occupied, what appeared vs what was absent. Without pattern direction, the extractor captures point-in-time readings. With it, it captures evidence that accumulates into fingerprints.

---

## Metric-Layer Anchoring

Beliefs are tied to the behavior of a metric or pattern over time — not to the document format that surfaced it.

The same business dynamic can surface whether it is discovered in a text bullet on a business review deck, a calculated cell in a financial model, a row in a database export, or a line item in a filing. The dynamic exists at the data layer, not the presentation layer.

This means:
- The belief reasoning prompt reads for metric movement and its interpretation — not for document structure
- A belief confirmed in a deck and a belief confirmed in a spreadsheet are treated identically
- New document types do not require new belief types — they are routed through the same extraction and reasoning logic after format-specific transcription
- The world model is **document-format-agnostic**. Only the intake method varies by format.

---

## The Silence Default Is Not a Weakness — It Is the Design

Every belief prompt instructs the engine to stay silent unless a durable interpretation is visible. Most documents produce no belief updates across most watch areas. This is correct behavior. The rarity of a genuine belief update is what keeps confidence meaningful.

A belief system that updates on everything is a summarizer.

---

## The Compounding Effect

After 3 documents: thin, uncertain beliefs at confidence 0.20–0.40.
After 10 documents: solidifying patterns at 0.50–0.70.
After 30+ documents: high-confidence structural understanding at 0.75+.

The world model becomes more valuable the longer it runs. This is the behavior of understanding, not retrieval.
