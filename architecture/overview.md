# Architecture Overview — Belief in the Stack

## The Four-Layer Architecture

Belief is built in four layers. Each layer has a distinct responsibility and runs at a different time.

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 0 — ENTITY FOUNDATION                                  │
│  foundation.md                                               │
│  Business model · Profitability thesis · Thesis metrics       │
│  Normalization model · Narration design · Signals vs noise    │
│  Runs: once per entity, before any belief stream             │
└──────────────────────────┬───────────────────────────────────┘
                           │  (read by every belief stream for this entity)
                           ▼
┌──────────────────────────────────────────────────────────────┐
│  LAYER 1 — CONFIGURATION                                      │
│  Strategic Blueprint                                          │
│  Who the entity is · what angle · candidate belief seed set   │
│  What patterns look like · what a belief looks like here      │
│  Runs: once per belief stream at setup time                   │
└──────────────────────────┬───────────────────────────────────┘
                           │  (read once, flows into both compilers)
            ┌──────────────┴──────────────────┐
            ▼                                  ▼
┌─────────────────────────┐       ┌──────────────────────────┐
│  LAYER 2 — COMPILATION  │       │  LAYER 2 — COMPILATION   │
│  Belief Reasoning Prompt│       │  Fact Extraction Prompt  │
│  (angle-aware,          │       │  (foundation-grounded,   │
│   foundation-grounded)  │       │   pattern-direction-aware)│
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

The foundation (Layer 0) grounds the blueprint (Layer 1), which is compiled into the runtime prompts (Layer 2). Every downstream step inherits its quality from this two-tiered cascade. If the foundation is precise and entity-specific, everything below it is precise. If the blueprint is correct, the execution prompts are correct without additional configuration.

Quality is injected at Layers 0 and 1 and propagates forward. Debugging a bad belief output should start by examining the foundation and blueprint, not the execution prompt.

### What Changes Cascade

A change to the blueprint automatically propagates to:
- The belief reasoning prompt (Layer 2A)
- The fact extraction prompt (Layer 2B)
- The belief memory (Layer 3)

A refinement to the foundation propagates to every belief stream for that entity.

No code changes required. The compilers are generic. Only the foundation and blueprint are entity-specific.

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
│         Belief Memory (L1) — living belief memory         │
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

Agents don't query Belief at runtime. The belief memory is **loaded into context at session start** — the same way a senior analyst reads their notes before a meeting. Every inference is then implicitly grounded by accumulated business understanding.

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

### Foundation — runs once per entity

```
Prompt -1 ← (user interview: business model, thesis metrics, normalization, narration, signals/noise)
          → entities/{entity_id}/foundation.md
```

### Setup — runs once per belief stream

```
Prompt 00 ← (entity, document types, angle, prior knowledge)
          → document_profile.md

Prompt 01 ← (foundation.md, document_profile.md, stated purpose)
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

The belief engine at runtime receives only its compiled prompt, the existing belief memory, and the fact log. It never receives the raw document, the foundation, or the blueprint. This is the contract that makes the system reproducible and auditable.

---

## The Storage Layers

| Layer | Purpose | Mutability |
|-------|---------|------------|
| **Entity Foundation** | Business understanding for the entity — thesis, metrics, normalization, narration. One `foundation.md` per entity. | Updated when streams reveal deeper understanding |
| **L1 — Belief Memory** | Living belief memory. One `belief.md` per stream. Loaded into agent context. | Surgically updated per document |
| **L2 — Fact Logs** | Per-document extracted signals. | Written once per document |
| **L3 — Raw Archive** | Original document content verbatim. | Immutable. Written once. |

---

## Pattern Recognition Is the Method, Not an Eighth Stream

Pattern recognition is not a separate belief stream. It is the method that all seven streams apply. Every stream is about patterns — the difference is what form patterns take:

| Stream | Pattern Form |
|--------|-------------|
| 00 — Factual Understanding | Metric definition stability; benchmark comparison consistency |
| 01 — Business Model Understanding | Structural margin and unit-economics trajectory across periods |
| 02 — Business Dynamics | Lead-lag pair recurrence; conversion chain stability; stress and feedback sequencing |
| 03 — Causal Understanding | Attribution sequencing; gap between stated cause and what actually moved |
| 04 — Business Memory | Decision pattern recurrence across pressure episodes |
| 05 — Narrative Understanding | Linguistic fingerprints; sequencing habits; what appears vs what is absent |
| 06 — Forecast and Plan Behavior | Revision direction and magnitude recurrence; planning bias stability |

The fact extraction prompt is the injection point for pattern direction. When the blueprint specifies what form patterns take, the fact extractor captures not just values but recurrence signals — the actual language used, the structural positions occupied, what appeared vs what was absent. Without pattern direction, the extractor captures point-in-time readings. With it, it captures evidence that accumulates into fingerprints.

---

## The Silence Default Is Not a Weakness — It Is the Design

Every belief prompt instructs the engine to stay silent unless a durable interpretation is visible. Most documents produce no belief updates across most beliefs. This is correct behavior. The rarity of a genuine belief update is what keeps confidence meaningful.

A belief system that updates on everything is a summarizer.

---

## The Compounding Effect

After 3 documents: thin, uncertain beliefs at confidence 0.20–0.40.
After 10 documents: solidifying patterns at 0.50–0.70.
After 30+ documents: high-confidence structural understanding at 0.75+.

The belief memory becomes more valuable the longer it runs. This is the behavior of understanding, not retrieval.
