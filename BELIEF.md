# Belief — Full Specification

*Business Belief Intelligence: How an AI system accumulates business understanding the way a senior analyst does — across any document, any business, any dimension.*

*Gogul Kumar Mathi · Corporate Finance AI · Expedia Group · 2026*

---

## 01 — What Is Belief

### The Problem Every AI System Has

Every agent you build operates on what is in its context window right now. The moment the session ends the understanding is gone. Next time it starts from zero. That is not intelligence. That is a very fast search engine.

The missing layer is this: how does an AI system accumulate understanding over time the way a human expert does? Not retrieve — understand. Not search — remember. Not summarize — believe.

### What Belief Is

Belief is a business belief intelligence system. It reads any document — decks, transcripts, reports, audio, PDFs, markdown — and builds a living model of how a business operates. Not what any single document says. The pattern of behavior visible only across many documents over time.

It does not store facts. It accumulates beliefs. And beliefs are different.

**Facts vs Beliefs**

- A fact: Revenue was $2.3B in October.
- A belief: Revenue growth is increasingly dependent on pricing rather than volume, suggesting the business is extracting more from its existing base rather than expanding it.

Facts are static. Beliefs carry direction, confidence, and trajectory. Beliefs drive intelligent reasoning. Facts just answer lookup queries.

### The Analyst Analogy

A senior analyst who has been in the room for ten months does not remember every slide. They hold a mental model of how the business behaves. They know what is seasonal and what is structural. They know when a number looks wrong. They know where to look when they need evidence.

Belief builds that mental model — from any document, for any business — and keeps it current as new documents arrive.

### The New Hire Who Never Learns

A capable new hire can do the task on day one but gets it wrong — they lack the context that takes months to absorb. Today's AI is that hire, except it never stops being new. Every task starts with no knowledge of how this place works. Belief ends that reset.

### What a Belief Is Not

| Not this | Why a belief is different |
|---|---|
| A **fact** to look up | A fact is retrieved and static. A belief is applied as working truth and can change. |
| A **hardcoded rule** | A rule is frozen at write-time. A belief is expected to drift and is maintained. |
| **Conversation or agent memory** | That remembers what *you said*. A belief remembers how *the business works*. |

The sharpest distinction is the last one. The field has invested heavily in giving AI memory of conversations and recent sessions. A belief is the other, harder kind — memory of the business itself.

### What a Belief Contains

For a given process, the belief layer captures the understanding an expert applies without thinking:

- **Definitions** — what each term means here (e.g., which definition of "revenue" applies in this room)
- **Trusted sources** — which system or file is the source of truth; which wins on conflict
- **What's normal** — expected ranges, so a real anomaly is distinguishable from noise
- **Process steps** — the sequence the work always follows
- **Rules and conventions** — how outputs are framed for different stakeholders

### Skills and Beliefs

A skill does the task — pulls the numbers, builds the report. It runs on whatever it can infer about the business at the time.

A belief is the understanding the task is performed *from* — the assumptions behind the doing. Skills take the doing; beliefs take the thinking.

Belief is scoped to the same process a skill already performs. It is not a model of the whole business. Start where a skill is running and capture the thinking behind that one process.

---

## 02 — The Belief

### A Belief Defined

A belief is a durable, falsifiable interpretation of how a business behaves — supported or challenged by evidence across many documents. It is not a fact. It is not a metric. It is not a slide summary. It is the compressed judgment that a trained analyst would carry in their head after months of reading.

### What Makes a Belief a Belief

- **Durable** — it holds true across time, not just this period
- **Falsifiable** — a future document can confirm or contradict it
- **Direction** — Improving, Stable, Deteriorating, or Unclear
- **Confidence** — a number from 0.05 to 0.95 reflecting how strongly it is held

### The Four Update States

| State | What Happened | Confidence Change |
|-------|---------------|-------------------|
| **New Prior** | No existing belief covers this signal | Seed at 0.20 |
| **Confirm** | New document supports this belief | +0.08 |
| **Contradict** | New document challenges this belief | −0.15 |
| **Decay** | Not seen in 90+ days | −0.05 per cycle |

Cap: 0.95 (0.90 for causal beliefs). Floor: 0.05. Archive below 0.10.

### The Fact-to-Belief Gate

Not everything in a document becomes a belief. Most things should produce silence. The gate is the filter.

| Gate State | Action |
|------------|--------|
| Fact only — a metric, a date, a result with no durable interpretation | Stay silent |
| Supports or challenges an existing belief but cannot create a new one | Update existing belief |
| Strengthens an existing belief without rewriting it | Confirm +0.08 |
| Challenges an existing belief | Contradict −0.15 |
| Implies something durable that no existing belief covers | New prior at 0.20 |

**Two gates every belief must pass before being written:**

1. **Falsifiability** — can a future document contradict this?
2. **Distinctiveness** — is this specific to this business, not any business in this sector?

---

## 03 — The Five Belief Types

Every document is read through all five belief types. Each type asks a different question about the same business. All five write into one world model — there is no separate memory per belief type.

### Business Memory
*What is structurally and durably true about how this business is built.*

Watches simultaneously across: Business Model & Structure, Business Dynamics, Growth Engine, Margin & Cost Behavior, Capital Allocation, Operating Scale & Leverage. The slowest belief to change. Updated rarely; high confidence when confirmed.

### Business Dynamics
*How the business behaves as a system — where pressure builds, where it releases.*

Watches: Cost-Revenue Relationship, Investment to Output Conversion, Volume and Price Dynamics, Feedback Loops, Organizational Response Sequence. How costs behave under pressure. Where efficiency builds or breaks.

### Narrative Understanding
*How the organization tells its story, and what the pattern of language reveals.*

Watches: Forward Commitment Pattern, Attribution Pattern, Language Specificity, Emphasis and Omission. Requires specific observable language anchors — not impressions. Never characterizes intent.

### Factual Understanding
*What has actually been observed and measured. The evidence layer.*

Not interpretation — the intake layer. Records revenue figures, growth rates, segment data, operating metrics, forward commitments, and signals being tracked. Most observations stay here. When a pattern recurs and an interpretation becomes visible, it moves into one of the other four belief types.

### Causal Understanding
*Which signals reliably predict which outcomes, with what lag.*

Each belief is one lead-lag pair: a leading signal, a lagging outcome, the observed lag, the count, and pending status. The only belief type that generates forward watch items — when a known leading signal appears, it is flagged immediately. Cap at 0.90. Requires both signal and outcome to be observed before a belief is written.

---

## 04 — The Lifecycle

### Three Layers of Storage

| Layer | What It Is | Characteristics |
|-------|-----------|-----------------|
| **L1 — World Model** | The living belief model | One file per belief type. Surgically edited — never rewritten. Loaded into context for every reasoning pass. |
| **L2 — Source Index** | One record per document | Stores time period, themes, metrics, narratives, anomalies. Used to find relevant documents without scanning raw content. |
| **L3 — Raw Archive** | The original document content | Immutable. Written once. Never reprocessed. Source of truth for evidence retrieval. |
| **Ledger** | Append-only audit log | Every document: signals extracted, beliefs affected, changes made. |

### Ingestion Pipeline — When a Document Arrives

```
DOCUMENT ARRIVES (any format)
          │
          ▼
┌─────────────────────────┐
│  DOCUMENT TYPE ID       │  pptx / pdf / docx / audio / md
└─────────────────────────┘
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
   FOR EACH BELIEF TYPE (01–05):
   ├─ Chunk 1: temp belief empty → dump all outputs
   └─ Chunk 2+: compare against temp belief
       confirm / contradict / merge / new / redundant / drifting
       End: temp belief = document-level belief
          │
          ▼
   MERGE INTO WORLD MODEL (L1)
   └─ surgical update per belief type
   └─ ledger entry written
   └─ decay pass runs
```

---

## 05 — The Prompt Architecture

Four prompts form a closed loop. None are static templates.

| Prompt | When It Runs | What It Does |
|--------|-------------|--------------|
| **Prompt 1 — Goal Alignment** | Once | ReAct interview loop. Discovers the business, documents, observation angle, known noise. Produces the master memory goal loaded into every subsequent pass. |
| **Prompt 2 — Document Intake** | Every upload | Inspects the file. Routes to vision, OCR, or direct extract. Writes raw extract to L3 and index entry to L2. |
| **Prompt 3 — Belief Reasoning** | Every chunk × every active belief type | The core intelligence prompt. Reads chunk + current world model + memory goal simultaneously. Applies the fact-to-belief gate. Makes surgical updates only. |
| **Prompt 4 — Decay & Maintenance** | After every ingestion cycle | Checks beliefs not seen in 90 days. Applies confidence decay. Archives below 0.10. Keeps the model bounded. |

The five belief type system prompts each implement Prompt 3 for one specific belief type. Every prompt shares identical action structure — the gate, the silence default, the surgical update logic, the direction field, the guardrails. Only the observation focus and worked examples differ.

---

## 06 — Why This Matters

### Three Things Belief Enables That Nothing Else Does

**01 — Contextual reasoning over time**
Every agent today is context-blind at session start. Belief gives agents a world model to reason from — not just a document to search through. Queries become calibrated to the business.

**02 — Belief-grounded evaluation**
Current evals check output format and SQL accuracy. Belief enables a new kind of eval — does this answer reflect what is actually true about this business? You can check agent outputs against the belief model.

**03 — Portable business understanding**
When you upgrade a model or rebuild a system the understanding currently lives nowhere. With Belief the world model is portable. A new model inherits the business understanding immediately. The investment compounds.

### The Business Case

**Saves repetitive thinking, not just repetitive doing.**
Skills removed the doing once. Beliefs remove the re-thinking — remember, decide, reconcile, frame — every time after. That is what compounds and what actually drives adoption.

**A durable asset, not a rented one.**
The model is rented and commoditized. The accumulated beliefs are owned and specific to this business. Investment in understanding does not depreciate when the model is swapped.

**The first step of interpretation.**
Running tasks inside a business is not the same as interpreting a business. Beliefs are the first step of interpretation — they make the system's outputs reflect how the business actually works, not just what was in the prompt.

### The Acceptance Criterion

Give Belief 10 months of business review decks. Read the resulting world model. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — the system is working.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## 07 — Concept Grounding

This is not a new architecture. It is a named implementation of an established idea.

- In **agent theory**, this is a *belief* in the BDI (Belief-Desire-Intention) sense — something the agent treats as true while acting.
- In **enterprise AI practice**, the same idea appears under different names: semantic memory, business rules engine, semantic layer, ontology, context layer.

Same underlying concept, different names across fields. Naming it honestly avoids overclaiming and makes it easier to reason about where it fits alongside tools teams already use.
