# Architecture Overview — Cortex in the AI Stack

## Where Cortex Sits

Cortex is not a retrieval system, a chat interface, or a database. It is the **reasoning layer** that sits above RAG and below the agent.

```
┌─────────────────────────────────────────────────────────┐
│                    AI AGENTS                            │
│         ODIN · THOR · Fury · Evals                     │
└──────────────────────┬──────────────────────────────────┘
                       │  belief context loaded at session start
                       ▼
┌─────────────────────────────────────────────────────────┐
│                   CORTEX LAYER                          │
│         World Model (L1) — 50–100 active beliefs        │
│         18 belief reasoning sub-lenses                  │
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

## How Agents Use Cortex

Agents do not query Cortex at runtime. The world model is **loaded into context at session start** — the same way a human analyst reads their notes before a meeting. Every inference is then implicitly grounded by accumulated business understanding.

### ODIN (Financial Query Agent)
Without Cortex: queries Hyperion blind — syntactically correct SQL, contextually naive answers.
With Cortex: knows Q3 always has a one-time EBITDA distortion. Knows a cost center runs 15% above plan historically. SQL becomes calibrated, not just correct.

### THOR (Narrative / Comms Agent)
Without Cortex: uses static voice profiles frozen at setup.
With Cortex: knows Scott has grown more cautious on margin guidance. Knows Ariane shifted from growth to efficiency framing six months ago. Scripts reflect evolving voice.

### Evals
Without Cortex: checks SQL accuracy and output format.
With Cortex: checks whether an answer reflects what is actually true about the business. A new category of evaluation that cannot exist without a belief layer.

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
         │   (Prompt 3 × 18)    │
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
         │  Bound to 50–100     │
         └──────────────────────┘
```

## The Three Storage Layers

| Layer | Purpose | Mutability |
|-------|---------|------------|
| **L1 — World Model** | Living belief model. Loaded into agent context. | Surgically updated per ingestion cycle |
| **L2 — Source Index** | One record per document: period, themes, metrics, anomalies. Used to route evidence retrieval. | Append-only |
| **L3 — Raw Archive** | Original document content verbatim. | Immutable. Written once. |
| **Ledger** | Append-only audit log of every belief change. | Immutable. Append-only. |

## The 18 Sub-Lenses as Parallel Reasoning Agents

Each of the 18 belief prompts is an independent reasoning agent watching a different dimension of the business. During ingestion, each active lens reads the same document chunk and produces its own surgical updates to its own section of the world model.

```
Document chunk
      │
      ├──► Lens 01: Business Model & Structure  ──► World Model: business_memory
      ├──► Lens 03: Growth Engine               ──► World Model: growth_engine
      ├──► Lens 07: Metric Movement & Anomaly   ──► World Model: bi_signals
      ├──► Lens 13: Narrative vs Reality        ──► World Model: narrative
      └──► ... (up to 18 active lenses)
```

Each lens enforces its own silence default — most inputs produce no update. The value is in what gets filtered out, not just what goes in.

## Why This Architecture Matters

### The Silence Default Is Not a Weakness — It Is the Design

Every belief prompt instructs the agent to stay silent unless a durable interpretation is visible. Most documents produce no belief updates across most lenses. This is correct behavior. The rarity of a genuine belief update is what keeps confidence meaningful.

### Surgical Updates Protect History

Beliefs are never overwritten. Contradictions are appended as notes. The world model accumulates evidence trails. A belief that has been contradicted twice is different from one that has never been challenged — and the system preserves that distinction.

### The Compounding Effect

After 3 documents: thin, uncertain beliefs at confidence 0.20–0.40.
After 10 documents: solidifying patterns at 0.50–0.70.
After 30+ documents: high-confidence structural understanding at 0.75+.

The world model becomes more valuable the longer it runs. This is the behavior of understanding, not retrieval.
