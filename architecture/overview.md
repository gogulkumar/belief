# Architecture Overview — Belief in the AI Stack

## The Three-Layer Architecture

Belief is built in three layers. Each layer has a distinct responsibility and runs at a different time.

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 1 — CONFIGURATION                                      │
│  Strategic Blueprint                                          │
│  Who the entity is · what angle · what watch areas           │
│  What patterns look like · what a belief looks like here      │
│  Runs: once at setup time                                     │
└──────────────────────────────┬───────────────────────────────┘
                               │  (read once, flows into both compilers)
              ┌────────────────┴─────────────────┐
              ▼                                   ▼
┌─────────────────────────┐        ┌──────────────────────────┐
│  LAYER 2 — COMPILATION  │        │  LAYER 2 — COMPILATION   │
│  Belief Reasoning Prompt│        │  Fact Extraction Prompt  │
│  (angle-aware,          │        │  (watch-area-scoped,     │
│   entity-grounded)      │        │   pattern-direction-aware)│
│  Runs: once per stream  │        │  Runs: once per document  │
└────────────┬────────────┘        └──────────────┬───────────┘
             │                                     │
             └──────────────┬──────────────────────┘
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

Belief is not a retrieval system, a chat interface, or a database. It is the **reasoning layer** that sits above RAG and below the agent.

```
┌─────────────────────────────────────────────────────────┐
│                    AI AGENTS                            │
│         ODIN · THOR · Fury · Evals                     │
└──────────────────────┬──────────────────────────────────┘
                       │  belief context loaded at session start
                       ▼
┌─────────────────────────────────────────────────────────┐
│                   BELIEF LAYER                          │
│         World Model (L1) — five belief types            │
│         Fact-to-belief gate · Update arithmetic         │
└──────────────────────┬──────────────────────────────────┘
                       │  evidence retrieval
                       ▼
┌─────────────────────────────────────────────────────────┐
│                    RAG / RETRIEVAL                      │
│         Source Index (L2) · Raw Archive (L3)            │
└──────────────────────┬──────────────────────────────────┘
                       │  documents
                       ▼
┌─────────────────────────────────────────────────────────┐
│                 DOCUMENT SOURCES                        │
│    Decks · Transcripts · Reports · Audio · PDFs · MD    │
└─────────────────────────────────────────────────────────┘
```

## How Agents Use Belief

Agents do not query Belief at runtime. The world model is **loaded into context at session start** — the same way a human analyst reads their notes before a meeting. Every inference is then implicitly grounded by accumulated business understanding.

### ODIN (Financial Query Agent)
Without Belief: queries Hyperion blind — syntactically correct SQL, contextually naive answers.
With Belief: knows Q3 always has a one-time EBITDA distortion. Knows a cost center runs 15% above plan historically. SQL becomes calibrated, not just correct.

### THOR (Narrative / Comms Agent)
Without Belief: uses static voice profiles frozen at setup.
With Belief: knows Scott has grown more cautious on margin guidance. Knows Ariane shifted from growth to efficiency framing six months ago. Scripts reflect evolving voice.

### Evals
Without Belief: checks SQL accuracy and output format.
With Belief: checks whether an answer reflects what is actually true about the business. A new category of evaluation that cannot exist without a belief layer.

## The Four-Component Model

```
┌──────────────────┐     ┌──────────────────┐
│   GOAL ALIGNMENT │     │  DOCUMENT INTAKE  │
│   (Prompt 1)     │     │  (Prompt 2)       │
│                  │     │                   │
│  ReAct interview │     │  Route → extract  │
│  Produces master │     │  Write L3         │
│  memory goal     │     │  Write L2 index   │
└────────┬─────────┘     └────────┬──────────┘
         │                        │
         └──────────┬─────────────┘
                    ▼
         ┌──────────────────────┐
         │   BELIEF REASONING   │
         │   (Prompt 3 × 5)     │
         │                      │
         │  Chunk + World Model │
         │  + Memory Goal       │
         │  → surgical update   │
         │  → ledger entry      │
         └──────────┬───────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │  DECAY & MAINTENANCE │
         │  (Prompt 4)          │
         │                      │
         │  90-day decay pass   │
         │  Archive < 0.10      │
         └──────────────────────┘
```

## The Three Storage Layers

| Layer | Purpose | Mutability |
|-------|---------|------------|
| **L1 — World Model** | Living belief model. One file per belief type. Loaded into agent context. | Surgically updated per ingestion cycle |
| **L2 — Source Index** | One record per document: period, themes, metrics, anomalies. Used to route evidence retrieval. | Append-only |
| **L3 — Raw Archive** | Original document content verbatim. | Immutable. Written once. |
| **Ledger** | Append-only audit log of every belief change. | Immutable. Append-only. |

## The Five Belief Types as Parallel Reasoning Agents

Each of the five belief type prompts is an independent reasoning agent watching a different dimension of the business. During ingestion, each active type reads the same document chunk and produces its own surgical updates to its own section of the world model.

```
Document chunk
      │
      ├──► Business Memory      ──► world_model/business_memory.md
      ├──► Business Dynamics    ──► world_model/business_dynamics.md
      ├──► Narrative Understanding ► world_model/narrative_understanding.md
      ├──► Factual Understanding ──► world_model/factual_understanding.md
      └──► Causal Understanding ──► world_model/causal_understanding.md
```

Each type enforces its own silence default — most inputs produce no update. The value is in what gets filtered out, not just what goes in.

## Why This Architecture Matters

### Pattern Recognition Is Cross-Cutting, Not a Sixth Type

Pattern recognition is not a separate belief type. It is the method that all five types apply. Every belief type is already about patterns — the difference is what form patterns take:

| Belief Type | Pattern Form |
|-------------|-------------|
| Business Memory | Cross-period behavioral recurrence; language the documents use to mark repetition |
| Business Dynamics | Structural ratio stability and trajectory across periods |
| Narrative Understanding | Linguistic fingerprints; sequencing habits; what appears vs what is absent |
| Factual Understanding | Metric definition stability; benchmark comparison consistency |
| Causal Understanding | Lead-lag pair recurrence; signal → outcome with consistent lag |

The fact extraction prompt is the injection point for pattern direction. When the blueprint (Layer 1) specifies what form patterns take for a given watch area, the fact extractor captures not just values but recurrence signals — the actual language used, the structural positions occupied, what appeared vs what was absent. Without pattern direction, the extractor captures point-in-time readings. With it, it captures evidence that accumulates into fingerprints.

The LLM does not need keyword lists. It needs direction. Direction is enough; pre-defined keywords are too brittle to survive across documents.

---

### Metric-Layer Anchoring

Beliefs are tied to the behavior of a financial metric over time — not to the document format that surfaced it.

The same business dynamic (e.g., "Loyalty Program Margin Dilution") surfaces whether it is discovered in a text bullet on a business review deck, a calculated cell in a financial model, a row in a database export, or a line item in an SEC filing. The dynamic exists at the data layer, not the presentation layer.

```
[ RAW BUSINESS METRIC ] ──► e.g., "HCOM Take Rate drops 40 bps"
              │
     ┌────────┴────────┐
     ▼                 ▼
[ DATA SOURCE ]   [ BELIEF LENS ]
 .pptx / .xlsx     "A 40 bps drop is consistent with
 .csv / .pdf        loyalty redemption dilution."
                        │
                        ▼
            [ EXTRACTED BUSINESS DYNAMIC ]
              "Loyalty Program Margin Dilution"
```

This means:
- The belief reasoning prompt reads for metric movement and its interpretation — not for document structure.
- A belief confirmed in a deck and a belief confirmed in a spreadsheet are treated identically.
- New document types do not require new belief types — they are routed through the same extraction and reasoning logic after Task 1 (format-specific extraction).
- The world model is **document-format-agnostic**. Only the extraction method in Task 1 of the ingestion pipeline varies by format.

### The Silence Default Is Not a Weakness — It Is the Design

Every belief prompt instructs the agent to stay silent unless a durable interpretation is visible. Most documents produce no belief updates across most types. This is correct behavior. The rarity of a genuine belief update is what keeps confidence meaningful.

### Surgical Updates Protect History

Beliefs are never overwritten. Contradictions are appended as notes. The world model accumulates evidence trails. A belief that has been contradicted twice is different from one that has never been challenged — and the system preserves that distinction.

### The Compounding Effect

After 3 documents: thin, uncertain beliefs at confidence 0.20–0.40.
After 10 documents: solidifying patterns at 0.50–0.70.
After 30+ documents: high-confidence structural understanding at 0.75+.

The world model becomes more valuable the longer it runs. This is the behavior of understanding, not retrieval.
