# Belief — Full Specification

*Business Belief Intelligence: How an AI system accumulates business understanding the way a senior analyst does — across any document, any business, any dimension.*

*Gogul Kumar Mathi · 2026*

---

## 01 — What Is Belief

### The Source

Every organization produces recurring documents as part of how it operates: quarterly business reviews, earnings calls, investor presentations, board decks, operating reports. These are not incidental outputs — they are the formal record of how management interprets performance, explains outcomes, and communicates direction.

What those documents contain, beyond the numbers:

- How this team explains performance — what causes they name, in what order, framed how
- Which metrics they treat as primary versus secondary
- How emphasis shifts when results disappoint
- What language recurs, and what language disappears
- How commitments are explained when they are not met

These behavioral patterns are not visible in any single document. They only emerge after reading many comparable documents from the same entity over time.

### The Analyst Analogy

A senior analyst who has been reading the same business's quarterly reviews for ten months holds a mental model that no one has written down. They know what a normal deck looks like for this business — so they immediately see what is unusual about this one. They know how this management team typically explains a shortfall. They know which commitment is always ambitious and which is typically conservative.

They do not remember every slide. They hold the pattern. That pattern is the belief.

That accumulated understanding is not in a file. It is not queryable. When the analyst leaves, it leaves with them.

### What Belief Is

Belief reads business process deliverables — the recurring decks, transcripts, reports, and documents that organizations already produce — and builds a living model of the behavioral patterns inside them.

Not what any single document says. The pattern of behavior visible only across many comparable documents over time.

It does not store facts. It accumulates beliefs. And beliefs are different.

**Facts vs Beliefs**

- A fact: Revenue was $2.3B in Q3.
- A belief: Revenue growth is increasingly dependent on pricing rather than volume, suggesting the business is extracting from its existing base rather than expanding it.

Facts are static. Beliefs carry direction, confidence, and trajectory. Beliefs tell you what to expect in the next document — and what would constitute a surprise worth investigating.

### What a Belief Is Not

| Not this | Why a belief is different |
|---|---|
| A **fact** to look up | A fact is retrieved and static. A belief is applied as working truth and can change. |
| A **document summary** | A summary describes what a document said. A belief describes what the pattern of documents means. |
| A **metric reading** | A metric is a value at a point in time. A belief is the durable interpretation of how a metric behaves over time. |
| A **management attribution** | Management saying "efficiency improved because of better process" is reported attribution. The belief is whether that attribution is consistent, how it is framed, and whether the numbers support it. |

### What a Belief Contains

A belief captures the understanding that a senior analyst holds implicitly after months of reading:

- **What is normal** — expected ranges and behaviors, so deviations are immediately visible
- **What patterns look like** — the specific recurring forms: language, structure, sequencing, attribution habits
- **What to expect next** — the forward signal that makes the belief actionable in the next document
- **What would be a surprise** — the invalidation signal that marks when the pattern has changed

### Skills and Beliefs

A skill does the task — pulls the numbers, builds the report. It runs on whatever it can infer about the business at the time.

A belief is the understanding the task is performed *from* — the working model behind the doing. Skills take the doing; beliefs take the thinking.

---

## 02 — The Belief

### A Belief Defined

A belief is a durable, falsifiable, actionable interpretation of how a business behaves — supported or challenged by evidence across many documents. It is not a fact. It is not a metric. It is not a slide summary. It is the compressed judgment that a trained analyst would carry in their head after months of reading.

### What Makes a Belief a Belief

- **Durable** — it holds true across time, not just this period
- **Falsifiable** — a future document can confirm or contradict it
- **Actionable** — it tells you what to expect next, and what would constitute a surprise worth investigating
- **Direction** — Improving, Stable, Deteriorating, or Unclear
- **Confidence** — a number from 0.05 to 0.95 reflecting how strongly it is held

### Also Not a Belief

Beyond the core distinctions (not a fact, not a rule, not agent memory), a belief is also not:

- A **metric reading** — a specific value at a specific point in time
- A **one-period observation** — something a single document mentioned that hasn't recurred
- A **document summary** — what a document said, not what it means
- An **opinion** about whether performance is good or bad
- A **projection or forecast** — a belief describes what is, not what is predicted

### The Dual Nature: Durable AND Provisional

A belief must be durable enough to act from — it should not be abandoned after one contradicting document. But it must also be provisional — open to revision when accumulated evidence warrants it.

These two properties are not in tension. They define the difference between a belief and either a reflex (no durability) or a bias (no provisionality).

Nir Eyal in *Beyond Belief* frames this precisely: the best beliefs are both practical and provisional — they offer just enough certainty to act, yet enough flexibility to adapt. A belief is a firmly held interpretation, open to revision when new evidence arrives. A belief that requires ignoring evidence to sustain itself is not a belief. It is a bias.

### Two Kinds of Belief Revision

Not all revision is the same.

**Incremental update** — The statement is refined as confidence rises or falls. The interpretive frame stays intact. A belief about "external attribution patterns" becomes more confident as three more comparable documents confirm it, or weakens as two consecutive documents show a different framing. The frame itself is stable.

**Perspective shift** — Enough evidence accumulates to suggest a fundamentally different interpretation of what has been observed. The frame itself changes. What looked like "cost efficiency behavior" was actually a response to a structural market constraint. What looked like "consistent external attribution" was a temporary framing choice that has now reversed. The prior evidence trail does not become invalid — it is now understood differently.

A perspective shift is not a failure of the previous belief. The previous belief was the best available interpretation at the time, held provisionally. A perspective shift is what a working belief model is designed to produce: accumulated evidence eventually changing not just confidence, but understanding.

### The Durability Ladder

A belief passes through maturity stages as more comparable documents are processed. Do not skip stages.

| Stage | Documents | What It Means |
|-------|-----------|---------------|
| **Candidate** | 1 | A signal with the shape of a durable pattern. Not yet a belief. Every first-document entry is Candidate. |
| **Provisional** | 2 | Two comparable documents support the pattern. Hold cautiously. |
| **Confirmed** | 3 | Three comparable documents support the pattern. Treat as a baseline. |
| **Established** | 4+ | The pattern is structural. Breaking it is meaningful signal, not noise. |

### The Four Update States

| State | What Happened | Confidence Change |
|-------|---------------|-------------------|
| **New Prior** | No existing belief covers this signal | Seed at 0.20 |
| **Confirm** | New document supports this belief | +0.08 |
| **Contradict** | New document challenges this belief | −0.15 |
| **Decay** | Not seen in 90+ days | −0.05 per cycle |

Cap: 0.95 (0.90 for causal beliefs). Floor: 0.05. Archive below 0.10.

These arithmetic updates handle incremental revision. They do not handle perspective shifts. When contradictions accumulate to the point where the interpretive frame itself is wrong — not just weakened — the belief engine must assess whether a reframe is warranted rather than continued decay. See Section 07 (Scope Boundaries) for when to retire vs reframe.

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

Every document is read through all five belief types. Each type asks a different question about the same business. All five write into one belief memory — there is no separate memory per belief type.

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

### Phase 0 — Seeding (Before Documents Arrive)

Before any document is ingested, the belief system should be seeded with a **Knowledge Dossier** — a human-written document capturing the institutional understanding of the business: how it makes money, where costs go, and what the normal operating ranges look like.

The dossier is not processed through the ingestion pipeline. A setup agent extracts candidate beliefs from it and writes them directly into the belief memory as seeded priors (confidence 0.20). From that point, documents confirm, contradict, or decay those beliefs exactly as they would any other belief.

This step turns the first document the system reads from a cold start into a calibrated one. See [`lifecycle/seeding.md`](../lifecycle/seeding.md) for the full Knowledge Dossier format, Metric-Dynamic Anchor table, and three-level cascade structure.

---

### Three Layers of Storage

| Layer | What It Is | Characteristics |
|-------|-----------|-----------------|
| **L1 — Belief Memory** | The living belief model | One file per belief type. Surgically edited — never rewritten. Loaded into context for every reasoning pass. |
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
   MERGE INTO BELIEF MEMORY (L1)
   └─ surgical update per belief type
   └─ ledger entry written
   └─ decay pass runs
```

---

## 05 — The Prompt Architecture

Two phases. Stream setup runs once. Document ingestion runs for every document.

### Phase 1 — Stream Setup (runs once per belief stream)

| Prompt | What It Does |
|--------|-------------|
| **Prompt 00 — Document Profile** | Interview agent. Discovers entity, document types, chosen angle, and prior knowledge. Produces a structured Document Profile. |
| **Prompt 01 — Strategic Blueprint** | Reads the Document Profile and produces the master configuration document. Defines what a belief means for this entity, this angle, these documents. Answered, not templated. |
| **Prompt 03 — Belief Reasoning Compiler** | Reads the blueprint and compiles a self-contained runtime system prompt for the belief engine. The belief engine uses this prompt at runtime — never the blueprint itself. |
| **Prompt 06 — Fact Extraction Compiler** | Reads the blueprint and compiled belief prompt and compiles the runtime system prompt for the fact extractor. One compiled extractor per document type. |

### Phase 2 — Document Ingestion (runs for every document)

| Component | What It Does |
|-----------|-------------|
| **intake.py** | Routes by format, transcribes, and splits into meaningful units. Writes immutable raw transcript to L3. |
| **fact_extractor.py** | Reads L3 units using the compiled fact extractor prompt. Extracts signals organized by watch area. Writes fact log to L2. Does not interpret — only captures. |
| **belief_engine.py** | Reads the fact log and the existing belief memory using the compiled belief reasoning prompt. Makes surgical updates to `belief.md`. Appends to `belief_changelog.md`. |

The cascade principle: quality injected at setup propagates forward without additional configuration. The belief engine at runtime receives only its compiled prompt, the existing `belief.md`, and the fact log. It has no other context — everything it needs is already inside the compiled prompt.

See [`lifecycle/ingestion-pipeline.md`](lifecycle/ingestion-pipeline.md) for the full step-by-step pipeline and runtime contract.

---

## 06 — Why This Matters

### The Three Use Cases

**01 — Pre-reading a document**
Before reading the next document, an analyst reads the belief first. It tells them what is already known to be true about how this entity communicates and performs. They walk in with priors, not cold. They read faster, notice more, and ask better questions.

**02 — Anomaly detection**
The belief sets the normal. When a new document arrives, the question is not "what does this document say?" but "what does this document do differently from what was expected?" Without a precise baseline, anomalies are invisible. With a belief, they surface immediately — not because a rule was triggered, but because the expected state is known.

**03 — Institutional memory transfer**
A new analyst reads the belief and gets years of behavioral context in minutes. For this to work, the belief must be specific enough to be actionable — not just "the business is seasonal" but specific enough that the analyst knows exactly what to look for in the next document, and what its absence would mean.

---

### Three Things Belief Enables That Nothing Else Does

**01 — Contextual reasoning over time**
Every agent today is context-blind at session start. Belief gives agents a belief memory to reason from — not just a document to search through. Queries become calibrated to the business.

**02 — Belief-grounded evaluation**
Current evals check output format and SQL accuracy. Belief enables a new kind of eval — does this answer reflect what is actually true about this business? You can check agent outputs against the belief model.

**03 — Portable business understanding**
When you upgrade a model or rebuild a system the understanding currently lives nowhere. With Belief the belief memory is portable. A new model inherits the business understanding immediately. The investment compounds.

### The Business Case

**Saves repetitive thinking, not just repetitive doing.**
Skills removed the doing once. Beliefs remove the re-thinking — remember, decide, reconcile, frame — every time after. That is what compounds and what actually drives adoption.

**A durable asset, not a rented one.**
The model is rented and commoditized. The accumulated beliefs are owned and specific to this business. Investment in understanding does not depreciate when the model is swapped.

**The first step of interpretation.**
Running tasks inside a business is not the same as interpreting a business. Beliefs are the first step of interpretation — they make the system's outputs reflect how the business actually works, not just what was in the prompt.

### The Acceptance Criterion

Give Belief 10 months of business review decks. Read the resulting belief memory. If a senior analyst reads it and identifies two or three beliefs they agree with that they would not have articulated explicitly — beliefs that feel true, that reflect how the business actually behaves — the system is working.

Not accuracy on a benchmark. Not a perplexity score. An analyst saying: *yes, that is what I know about this business.*

---

## 07 — Scope Boundaries

Every step in the pipeline has explicit prohibitions. These are not edge-case warnings — they are the core integrity rules that prevent the system from manufacturing beliefs it has not earned.

### What the Document Profile (Prompt 00) Must Never Do

- Create beliefs. The interview only captures intent.
- Interpret documents. If documents are available at setup, it profiles them — it does not extract signals.
- Decide what signals matter. That is the blueprint's job.
- Invent document characteristics the user did not describe.

### What the Strategic Blueprint (Prompt 01) Must Never Do

- Create beliefs. The blueprint is configuration, not extraction.
- State what beliefs will exist. It defines what beliefs CAN exist if signals recur — the difference matters.
- Invent entity vocabulary. If the user did not provide it and the documents did not contain it, it is not in the blueprint.
- Leave any section generic or templated. Every section must be answered for this entity.

### What the Fact Extractor Must Never Do

- Interpret signals. Capturing is not interpreting. The fact extractor surfaces what the document contains; the belief engine decides what it means.
- Summarize. A summary is not a fact log. The belief engine needs raw signals.
- Invent signals the document does not contain. "No signal in this window" is a valid and useful output.
- Extract signals the blueprint's signal matrix says the document type cannot carry.
- Create or update beliefs. It reads documents; it does not reason about them.

### What the Belief Engine Must Never Do

- Receive the raw document at runtime. It reads only the compiled belief reasoning prompt, the existing `belief.md`, and the fact log.
- Receive the blueprint at runtime. Everything it needs from the blueprint is already encoded in the compiled prompt.
- Invent evidence not present in the fact log. If the fact log does not contain it, the belief engine does not hold it.
- Write a belief from a single document. A first-document entry is Candidate only — explicitly marked as not yet a belief.
- Lower confidence precipitously on a single contradicting signal. One document showing tension does not revise an Established belief.
- Write beliefs that fail the quality test: non-falsifiable, entity-generic, single-period, template artifacts, unsupported causality.
- Confuse RETIRED with REFRAMED. RETIRED means the pattern ended — the behavior is no longer occurring. REFRAMED means the interpretive lens was wrong — the behavior was occurring but it meant something different than previously understood. When multiple consecutive TENSION signals accumulate, the engine must ask: did the pattern end, or did we misread what the pattern was?

### The Silence Default

Most documents produce no update. The gate is the mechanism. Rarity of genuine updates is what keeps confidence meaningful. A belief system that updates on everything is a summarizer.

---

## 09 — Concept Grounding

This is not a new architecture. It is a named implementation of an established idea.

- In **agent theory**, this is a *belief* in the BDI (Belief-Desire-Intention) sense — something the agent treats as true while acting.
- In **enterprise AI practice**, the same idea appears under different names: semantic memory, business rules engine, semantic layer, ontology, context layer.

Same underlying concept, different names across fields. Naming it honestly avoids overclaiming and makes it easier to reason about where it fits alongside tools teams already use.

---

## 10 — Open Design Questions

These are unresolved architectural decisions. They are documented here to prevent them from being re-litigated from scratch each time the system is extended.

**01 — Pattern fingerprint as a fifth belief field**
Should a dedicated field hold the concrete recurring signals (specific language, structural positions, behavioral markers) that evidence the belief? Currently, a belief states that a pattern exists. A fingerprint would hold what the pattern looks like in enough detail that the next document parser can check for it explicitly. Decision deferred: if enriching the blueprint layer produces fingerprints naturally inside the existing four fields, the fifth field is redundant. If the four fields remain descriptive rather than evidential, it is needed.

**02 — Anomaly detection as a first-class output**
Should the pipeline produce an explicit anomaly report when a new document breaks an established pattern? Currently the changelog records that a belief changed but not why it changed or what the anomaly was. A dedicated anomaly output would be more actionable for the pre-reading use case.

**03 — Cross-document pattern synthesis**
The pipeline processes one document at a time. Some patterns only emerge across many documents — seasonal behavior, systematic forecast bias, consistent framing shifts. Should there be a periodic synthesis step that looks across all fact logs simultaneously rather than evolving beliefs one document at a time?

**04 — Explicit durability tiers**
Should the Provisional / Confirmed / Established stages from the durability ladder be stored as explicit fields in the belief entry, or is the document count implicit in the evidence trail sufficient? Explicit tiers make the stage queryable; implicit tiers keep the format simpler.

**05 — Statement diff in changelog**
The changelog currently records that a statement changed, not what it said before vs after. Full statement history would enable drift analysis — understanding how the interpretation of a business evolved over time as more documents were read. Not yet implemented.

**06 — document_mechanics.json wiring**
The system can maintain a knowledge base of document-type structure by sector (`document_mechanics.json`) — what sections typically appear, in what order, what each section reliably carries. This would allow the interview agent (Prompt 00) and the blueprint compiler (Prompt 01) to pre-populate signal matrix defaults for common document types rather than deriving them from scratch each time. Open question: at what point in the setup flow does this knowledge base get consulted, and how does it interact with user-provided document samples that may deviate from sector norms? Not yet wired into the questioning session.
