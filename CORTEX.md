# Cortex — Full Specification

*Business Belief Intelligence: How an AI system accumulates business understanding the way a senior analyst does — across any document, any business, any dimension.*

*Gogul Kumar Mathi · Corporate Finance AI · Expedia Group · 2026*

---

## 01 — What Is Cortex

### The Problem Every AI System Has

Every agent you build — ODIN, THOR, Fury — operates on what is in its context window right now. The moment the session ends the understanding is gone. Next time it starts from zero. That is not intelligence. That is a very fast search engine.

The missing layer is this: how does an AI system accumulate understanding over time the way a human expert does? Not retrieve — understand. Not search — remember. Not summarize — believe.

### What Cortex Is

Cortex is a business belief intelligence system. It reads any document — decks, transcripts, reports, audio, PDFs, markdown — and builds a living model of how a business operates. Not what any single document says. The pattern of behavior visible only across many documents over time.

It does not store facts. It accumulates beliefs. And beliefs are different.

**Facts vs Beliefs**

- A fact: Revenue was $2.3B in October.
- A belief: Revenue growth is increasingly dependent on pricing rather than volume, suggesting the business is extracting more from its existing base rather than expanding it.
- Facts are static. Beliefs carry direction, confidence, and trajectory. Beliefs drive intelligent reasoning. Facts just answer lookup queries.

### The Analyst Analogy

A senior analyst who has been in the room for ten months does not remember every slide. They hold a mental model of how the business behaves. They know what is seasonal and what is structural. They know when a number looks wrong. They know where to look when they need evidence.

Cortex builds that mental model — from any document, for any business, across any dimension — and keeps it current as new documents arrive.

### Where This Matters for AI Engineering

Every enterprise AI deployment today is stateless. RAG retrieves facts but does not hold beliefs. It does not track trajectories. It does not notice when the story changes. Cortex is the layer that sits above RAG and below the agent — the reasoning layer that turns document retrieval into business understanding.

**Three systems that change with Cortex**

- **ODIN** — currently queries Hyperion blind. With Cortex beliefs in context it knows Q3 always has a one-time EBITDA distortion, that a cost center runs 15% above plan historically. SQL becomes calibrated not just correct.
- **THOR** — currently uses static voice profiles. With Cortex narrative memory it knows Scott has grown more cautious on margin guidance and Ariane shifted from growth to efficiency framing six months ago. Scripts reflect evolving voice not frozen profiles.
- **Evals** — currently check SQL accuracy. With Cortex you can check whether an answer reflects what is actually true about the business. A new kind of eval that does not exist without a belief layer.

---

## 02 — The Belief

### A Belief Defined

A belief is a durable, falsifiable interpretation of how a business behaves — supported or challenged by evidence across many documents. It is not a fact. It is not a metric. It is not a slide summary. It is the compressed judgment that a trained analyst would carry in their head after months of reading.

### What Makes a Belief a Belief

- **Durable** — it holds true across time, not just this period
- **Falsifiable** — a future document can confirm or contradict it
- **Direction** — Improving, Stable, Deteriorating, or Unclear
- **Confidence** — a number from 0 to 1 reflecting how strongly it is held
- **Evidence** — a count of how many documents have confirmed it
- **Provenance** — every belief traces to the document and unit that created it

### What a Belief Looks Like

```
GROWTH ENGINE
Revenue growth is increasingly dependent on pricing rather than volume,
suggesting the business is extracting more from its existing base rather than expanding it.
Confidence: 0.74  |  Evidence: 5 documents  |  Direction: Deteriorating

NARRATIVE VS REALITY
Revenue misses are consistently framed as timing issues, but the same demand weakness
recurs the following period, suggesting the explanation is structural not temporal.
Confidence: 0.81  |  Evidence: 7 documents  |  Direction: Deteriorating

BUSINESS DYNAMICS
Marketing spend increases precede revenue recovery by one to two periods,
revealing a meaningful lag between investment and return in the growth engine.
Confidence: 0.63  |  Evidence: 3 documents  |  Direction: Stable

FORECAST RELIABILITY
H2 revenue plans are consistently set above what the business delivers,
with actuals landing 5–8% below H2 plan in three of four years.
Confidence: 0.80  |  Evidence: 6 documents  |  Direction: Deteriorating
```

### The Four Update States

Every time a new document arrives, each belief in the world model can do one of four things. This is the lifecycle of a belief.

| State | What Happened | What Changes |
|-------|---------------|--------------|
| **Confirmed** | New document supports this belief | Confidence +0.08, Evidence +1, Last seen updated |
| **Contradicted** | New document challenges this belief | Confidence −0.15, Volatility flagged, Contradiction note appended |
| **New Prior** | No existing belief covers this signal | New line added at Confidence 0.20, Evidence 1 |
| **Decayed** | Not seen in 90+ days | Confidence −0.05 per cycle, archived below 0.10 |

### The Fact-to-Belief Gate

Not everything in a document becomes a belief. Most things should be ignored. The gate is the filter that separates durable interpretation from noise.

| Gate State | Definition | Action |
|------------|------------|--------|
| **Fact Only** | A metric, a date, a quote, a result — with no durable interpretation | Skip it |
| **Evidence Only** | Supports or challenges an existing belief but cannot create a new one on its own | Update existing belief |
| **Confirms Pattern** | Strengthens an existing belief without rewriting it | Confirm +0.08 |
| **Contradicts Pattern** | Challenges an existing belief. Adds contradiction evidence — does not erase history | Contradict −0.15 |
| **Durable Interpretation** | Implies something durable no existing belief covers. May create one new belief | New prior at 0.20 |

---

## 03 — The Three Belief Buckets

All beliefs live under one of three parent buckets. The user picks a bucket first. The system then identifies the right sub-lens within it based on the user's focus. The bucket defines what the agent is watching. The sub-lens defines the exact angle.

### BUSINESS MEMORY
*What is durably true about how this business operates as a system.*

| # | Sub-Lens | Observation Focus |
|---|----------|------------------|
| 01 | Business Model & Structure | What is permanently true about how this business is built and makes money |
| 02 | Business Dynamics | How the parts of the business affect each other — leads, lags, loops, leverage |
| 03 | Growth Engine | What actually drives top-line growth and whether it is sustainable |
| 04 | Cost & Efficiency Behavior | How costs move relative to output and where efficiency builds or breaks |
| 05 | Strategic Priorities & Investment | Where real resources go vs what is stated as a priority |
| 06 | Scale & Organizational Efficiency | Whether the organization becomes more or less efficient as it grows |

### BI SIGNALS
*What is moving, anomalous, or recurring right now. The live pulse.*

| # | Sub-Lens | Observation Focus |
|---|----------|------------------|
| 07 | Metric Movement & Anomaly | What metrics are trending, deviating, or recurring as signals |
| 08 | Operating Risk | Recurring fragilities that keep appearing unresolved |
| 09 | Forecast Reliability | Whether plans and guidance are becoming more or less trustworthy |
| 10 | People, Talent & Organizational Health | Capability, attrition, capacity, and leadership stability patterns |
| 11 | Execution Consistency | Whether initiatives complete or recycle without resolution |
| 12 | Early Warning Signals | Which signals reliably appear before the business feels the impact |

### NARRATIVE / STORYLINE
*How the story is told and where it diverges from the numbers.*

| # | Sub-Lens | Observation Focus |
|---|----------|------------------|
| 13 | Narrative vs Reality | Where language diverges from what the numbers show |
| 14 | Management Credibility | Whether leadership delivers what it commits to |
| 15 | External Attribution | Whether macro blame is selective or genuine |
| 16 | Emphasis & Omission | What the documents choose to foreground and what they quietly drop |
| 17 | Tone & Confidence Shift | How leadership language is changing direction over time |
| 18 | Competitive Positioning Narrative | How the business describes its own competitive standing and whether that is shifting |

---

## 04 — The Lifecycle

### Three Layers of Storage

| Layer | What It Is | Characteristics |
|-------|-----------|-----------------|
| **L1 — World Model** | The living belief model | 50–100 active belief lines per category. Surgically edited — never rewritten. Loaded into context for every reasoning pass. |
| **L2 — Source Index** | One record per document | Stores time period, themes, metrics, narratives, anomalies. Used to find relevant documents for a query without scanning raw content. |
| **L3 — Raw Archive** | The original document content | Immutable. Written once. Never reprocessed. Source of truth for deep evidence retrieval. |
| **Ledger** | Append-only audit log | Every document: signals extracted, beliefs affected, changes made. Full audit trail. |

### Ingestion Pipeline — When a Document Arrives

```
DOCUMENT ARRIVES (any format)
          │
          ▼
┌─────────────────────────┐
│  DOCUMENT TYPE ID       │  pptx / pdf / docx / audio / md
└─────────────────────────┘
    │           │         │
    ▼           ▼         ▼
 VISION        OCR      DIRECT
(pptx/img)  (scanned)  (text/md)
          │
          ▼  RAW EXTRACT → L3 (once only, never rerun)
          │
          ▼
┌──────────────────────────┐
│  IS DOCUMENT < 50k TOK?  │
└──────────────────────────┘
    YES → single belief pass
    NO  → chunk loop (50k per chunk)
          │
          ▼
   CHUNK LOOP (per 50k, per category)
   ├─ Chunk 1: temp belief empty → dump all outputs
   └─ Chunk 2+: compare against temp belief
       confirm / contradict / merge / new / redundant / drifting
       End: temp belief = document-level belief
          │
          ▼
   MERGE INTO WORLD MODEL (L1)
   └─ surgical update per category
   └─ ledger entry written
   └─ decay pass runs
```

### The Temp Belief Maintenance Loop

When a document exceeds the token limit it is processed in chunks. Each chunk produces belief outputs. Those outputs are compared against what the previous chunk already produced — to prevent redundancy, resolve drift, and keep the belief set clean before it gets written to the world model.

| State | What Happened | Resolution |
|-------|---------------|------------|
| **Overlapping** | Two chunks produced the same belief in different words | Merge into one |
| **Redundant** | A chunk produced a belief already held at high confidence | Discard |
| **Drifting** | A chunk produced a belief similar to but subtly different from an existing one | Flag for review, suggest merge or split |
| **Contradicting** | A chunk produced a belief that challenges one produced by an earlier chunk of the same document | Hold both, flag tension |
| **New** | A chunk produced a belief that has no match in the temp set | Add it |

---

## 05 — The Prompt Architecture

There are four prompts in Cortex. None are static templates. They are generated from the user's setup conversation and adapt to the document type and business domain.

| Prompt | When It Runs | What It Does |
|--------|-------------|--------------|
| **Prompt 1 — Goal Alignment** | Once | A ReAct interview loop — one question at a time. Discovers the business, the documents, the observation angle, the noise, and the wrong-belief cost. Produces the master memory goal loaded into every subsequent pass. |
| **Prompt 2 — Document Intake** | Every upload | Inspects the file. Routes to vision, OCR, or direct extract. Executes the intake. Produces the raw extract stored in L3 and the index entry written to L2. |
| **Prompt 3 — Belief Reasoning** | Every chunk × every active category | The core intelligence prompt. Reads chunk + current world model + memory goal simultaneously. Applies the fact-to-belief gate. Makes surgical updates only. Writes the ledger entry. |
| **Prompt 4 — Decay & Maintenance** | After every ingestion cycle | Checks beliefs not seen in 90 days. Applies confidence decay. Archives below 0.10. Reports volatility. Keeps the model bounded at 50–100 active beliefs. |

The 18 belief reasoning system prompts each implement Prompt 3 for one specific sub-lens. Every prompt shares identical action structure — the gate, the silence default, the surgical update logic, the direction field, the guardrails. Only the observation focus and worked examples differ.

---

## 06 — The Cowork Application

### Why Cowork

Cortex is a multi-step, multi-tool, file-in file-out workflow that runs across many documents over time. It has a loop inside a loop — the chunk loop inside the document loop. It passes files between tasks. It has a configurable parameter that changes how many iterations it runs. That is too complex for a single conversation. It needs a persistent task runner with file access.

### The Seven Task Sequence

| Task | Name | Input → Output |
|------|------|----------------|
| 1 | **Identify Document Type** | File → raw transcription + document type label |
| 2 | **Select Belief Prompts** | Config → ordered list of prompts to run |
| 3 | **Chunk the Transcription** | Raw text → ordered list of chunks with sequence numbers |
| 4 | **Belief Extraction Loop** | Chunks × categories → temp belief per category |
| 5 | **Temp Belief Maintenance** | Temp belief → clean temp belief (overlaps resolved) |
| 6 | **Document-Level Belief Finalization** | Clean temp → final document-level belief file per category |
| 7 | **Merge Into World Model** | Document belief + world model → updated world model + ledger entry |

---

## 07 — Why This Matters

### What Separates a Smart Tool from an Intelligent System

A smart tool answers questions correctly. An intelligent system understands the context those questions live in. The difference is the belief layer — the accumulated understanding that makes every query smarter than the last.

### The Three Things Cortex Enables That Nothing Else Does

**01 — Contextual reasoning over time**
Every agent today is context-blind at session start. Cortex gives agents a world model to reason from — not just a document to search through. Queries become calibrated to the business not just syntactically correct.

**02 — Belief-grounded evaluation**
Current evals check output format and SQL accuracy. Cortex enables a new kind of eval — does this answer reflect what is actually true about this business? You can check agent outputs against the belief model. That is a quality bar that does not exist without this layer.

**03 — Portable business understanding**
When you upgrade a model or rebuild a system the understanding currently lives nowhere — it has to be re-engineered from scratch. With Cortex the world model is portable. A new model inherits the business understanding immediately. The investment compounds instead of resetting.

### The Honest Risk

This only matters if belief quality is high. If Cortex produces generic beliefs that any finance system would generate — it adds complexity without value. The belief quality comes entirely from the prompt engineering. The 18 system prompts, the questioning agent, the fact-to-belief gate, the worked examples — that is the hard part. And it is what makes Cortex specific rather than generic.

### The Acceptance Criterion

Give Cortex 10 months of HCOM business review decks. Read the resulting world model. If a senior HCOM analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — the system is working.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*
