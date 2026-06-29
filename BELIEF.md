# Belief — Full Specification

*Institutional memory is portable: what is normal, what is template, what is evolving, and what would break — loaded into agents at the moment they draft, validate, or flag — instead of re-teaching the model from raw decks every time.*

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

They know that when the deck leads with a small beat, the real story is always in the second half of the bridge. They know that when the word "investment" appears in the cost commentary, it means discretionary spend the team chose — not external pressure. They know that the Q2 sequential pattern in this business always looks like a softening that recovers in Q3, and that certain numbers are structurally low because of how the business closes its quarter.

None of that knowledge lives in any single document. It accumulated across dozens of documents, hundreds of reviews, and years of pattern recognition. That analyst is the most valuable person in the room during a business review. And any AI deployed against the same documents starts each session knowing nothing that she knows.

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

### The Failure Modes Beliefs Prevent

These are the failure modes that appear when AI works with recurring business documents without institutional memory.

**The summary trap.** AI tools produce accurate summaries of individual documents. But a summary of what this document says is not the same as knowing what this document means in the context of everything that came before it. The belief system does not summarise. It maintains standing interpretations that exist independently of any single document.

**The attribution fabrication trap.** AI models are fluent with causal language. They connect observations to explanations with confidence. "Revenue declined because of FX headwinds" — but was it? Or did the team attribute it to FX because FX is an uncontrollable external factor that absolves accountability for a controllable miss? The belief system separates management's stated attribution (what they said caused it) from verified causality (what actually caused it). These are different beliefs, tracked separately, evolved separately.

**The recency trap.** An AI reading this month's deck in isolation treats everything in it as equally newsworthy. A senior analyst who has read 18 months of decks knows that three of the five items in this month's bridge are structural recurring patterns — not news — and the one genuinely new signal is buried in a footnote on slide 12. The belief system creates that distinction mechanically: what is expected (held as a belief) versus what is new (not yet matched to any belief, or contradicting a held belief).

**The vocabulary drift trap.** Business teams change the language they use to describe things over time — sometimes deliberately, sometimes not. When a word disappears from commentary, it might mean the thing stopped happening, or it might mean the team stopped wanting to highlight it. When a new phrase appears, it might reflect a genuine strategic shift or a narrative management choice. A system that tracks beliefs about language patterns surfaces vocabulary drift as a signal. A system that just reads the current document does not notice.

### What a Belief Contains

The belief memory carries four kinds of institutional understanding:

| What it captures | Where it lives in the system | What it gives you |
|---|---|---|
| **What is normal** | Normal baseline (belief entry field) | Expected ranges and behaviors — deviations are immediately visible without re-reading the archive |
| **What is template** | Entity foundation — signals-vs-noise | What this entity always says, what language is formulaic, what to discount so the real signal stands out |
| **What is evolving** | Evolution trail (belief entry field) | Per-document history of how the pattern developed — deepened, tensioned, narrowed, or shifted perspective |
| **What would break** | Falsification test (belief entry field) | The specific signal in a future document that would contradict, narrow, or retire the belief |

These four things — loaded into an agent at the start of a drafting, validation, or flagging task — replace the need to re-teach the model from raw decks every time. The belief memory is the institutional knowledge; the raw documents are the evidence it was built from. Once the belief exists, the document does not need to be re-read.

Each belief entry has five fields: **Statement**, **Why it matters**, **Evolution trail**, **Normal baseline**, **Falsification test**.

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

### What a Mature Belief Looks Like

After five or more comparable documents, a belief entry looks like this. Notice that the heading is a complete, specific, falsifiable sentence — not a category label.

---

**## Belief #7 — Every Variance Headline Opens With the Best-Performing Driver; the Negative Is Always the Second Clause**
**Status:** Established | **Confidence:** 0.60 | **Direction:** Stable

**Statement:** Established across five documents. Every variance headline in every deck is structured positive-first without exception. Whether the overall result is a beat or a miss, the opening attribution clause names a positive driver. In beats, the leading positive is the dominant contributor. In misses or mixed results, the team still locates a positive to lead with before acknowledging the headwind. The negative driver is structurally relegated to the second clause in every instance across all five documents. This is a systematic narrative convention, not a neutral factual ordering.

**Why it matters:** A reader who knows this pattern knows to find the actual commercial story in the offset clause, not the headline. A reader who does not know this pattern will systematically overweight the opening attribution because it leads.

**Evolution trail:** First seen in Document 1 — the headline led with channel efficiency ahead of a modest miss; I noted it but held it as a single observation. Document 2 repeated the same structure: a positive driver led even though the result was flat. At that point I held it as Provisional. Documents 3, 4, and 5 each confirmed the same structure without exception, including one document where the business posted its weakest result of the period — and still led with the best available positive. Established.

**Normal baseline:** In the next comparable document, the first clause of every variance headline will name the most favourable driver available, regardless of whether the overall result is positive or negative.

**Falsification test:** A headline that opens with a miss, headwind, or structural deterioration before stating any positive contributor would indicate a break in the team's narrative sequencing convention and should be investigated immediately.

---

A mature belief stream holds 12–20 entries at this level of specificity. Together they cover what is structurally established, what is under tension, and what is being watched as a Candidate. Any analyst — or any AI model — loading the full stream before opening the next document is informed in ways that are impossible to replicate by reading that document alone.

---

## 03 — The Five Belief Types

Every document is read through all five belief types. Each type asks a different question about the same business. All five write into one belief memory — there is no separate memory per belief type.

### Learning How Finance Consistently Explains Itself
*What behavioral and communication patterns has this team established across recurring documents.*

Watches: attribution sequencing, framing of beats and misses, vocabulary choices, commitment language, emphasis and omission patterns, how emphasis shifts when results disappoint. The belief type most sensitive to vocabulary drift. Updated when language patterns change.

### Learning How This Business Generates and Loses Value
*Not what the numbers are — what the numbers mean for how the business structurally works.*

This is the stream for understanding numbers. Not reading them — understanding them.

"Learning What Finance Has Recorded as True" tells you what the numbers are: the value, the period, the comparison. This stream tells you what the numbers mean: ratio relationships, mechanics, what moves what, what the number reveals about the thesis.

**What it specifically learns:**

**Ratio relationships.** Not just the ratio — but what movement in the ratio means for the business. When a thesis-defining ratio compresses, is that a quarter problem or a business model signal? Business Dynamics holds the interpretation of what the ratio means, not just its value.

**Lead-lag mechanics.** The number you see today predicts the number you will see next quarter. When Q1 spend peaks, Q2 bookings recover. These lead-lag relationships are invisible unless you have read enough documents to see the pattern. Business Dynamics holds them.

**Normal ranges.** Not the plan target — the realistic operating range given the business model. A number that looks alarming in isolation might be structural for this business at this time of year. Business Dynamics holds what normal looks like so a deviation is immediately visible.

**Structural mechanics.** Take rate, contribution margin, revenue conversion. How the business turns an input into an output. The chain between booking and revenue and profit, and where it breaks under pressure.

**Metric relationships.** When two metrics diverge, the divergence is the signal. When volume is flat but revenue grows, that is price-driven growth. When both grow together, that is volume expansion. The relationship between metrics tells you more than either metric alone — and Business Dynamics holds the interpretation of what the relationship means for this business.

**How this changes what an agent produces:**

Without Business Dynamics loaded:
> "S&M/NBV is 32%, 200bps above the FC target."
> That is a fact answer. It reads the slide.

With Business Dynamics loaded:
> "S&M/NBV is 32%, 200bps above FC target. This is within the expected Q1 range — this business runs a deliberate spend-ahead-of-season mechanic where Q1 compression normalizes in Q2. This is the thesis executing, not a problem. Watch Q2 recovery against FC."
> That is a judgment answer. It reads the slide and knows what it means.

The number did not change. The understanding of the number changed — because the Business Dynamics belief carried the context that turned a ratio into meaning.

### Learning How Documents Frame the Story
*How the organization tells its story, and what the pattern of language reveals.*

Watches: Forward Commitment Pattern, Attribution Pattern, Language Specificity, Emphasis and Omission. Requires specific observable language anchors — not impressions. Never characterizes intent.

### Learning What Finance Has Recorded as True
*What the numbers ARE — stable readings, verified values, benchmark comparisons.*

This is the evidence layer. Not interpretation — intake. It records what a document actually stated: the metric, the value, the period, the comparison benchmark. "NBV was $1.2B in May vs Plan of $1.1B." That is a factual belief.

Most observations from any document stay here. When a pattern recurs across multiple comparable documents and an interpretation becomes visible, it moves into one of the other streams.

The distinction from Business Dynamics: Factual Understanding records the reading. Business Dynamics holds the understanding of what that reading means for how the business structurally works. Both are needed. Factual without Dynamics produces an agent that reads the number correctly but cannot say what it means. Dynamics without Factual produces interpretation without a verified evidence base.

### Learning What Kind of Business This Actually Is
*What is the durable structural truth about what this business is, how it makes money, and why it performs the way it does.*

Watches: business model and profitability thesis, growth engine and its primary levers, structural cost behavior, capital allocation pattern, operating scale and leverage dynamics. The slowest belief to change — only updates when structural understanding shifts. High confidence when confirmed across many documents.

---

## 04 — The Lifecycle

### Step −1 — Entity Foundation (Before Any Stream)

Before any belief stream is created, the entity foundation must exist. The foundation is built once per entity through a structured interview (Prompt −1) and stored at `entities/{entity_id}/foundation.md`.

The foundation captures five things: the business model and profitability thesis, the thesis-defining metrics, the normalization model (what normal looks like for each metric), the narration design (how this entity communicates), and what matters vs. noise in its documents.

Every belief stream for the entity inherits the foundation as its prior. Without it, streams produce document-level observations. With it, they produce grounded business judgment — beliefs connected to the thesis, not floating.

The foundation is a living document. As streams accumulate deeper understanding, the foundation can be refined, and the refinement propagates to every stream for that entity.

---

### Three Layers of Storage

| Layer | What It Is | Characteristics |
|-------|-----------|-----------------|
| **Entity Foundation** | Business understanding for the entity | One `foundation.md` per entity. Built before any stream. Referenced by every blueprint for that entity. |
| **L1 — Belief Memory** | The living belief file for one stream | One `belief.md` per stream. Numbered beliefs, surgically updated. Loaded into context at session start. |
| **L2 — Fact Logs** | Per-document extracted signals | One fact log per document per stream. Written once. Read by the belief engine. |
| **L3 — Raw Archive** | Original document content | Immutable. Written once. Never reprocessed. Source of truth for evidence retrieval. |
| **Changelog** | Append-only audit trail | Every document: which beliefs changed, what action, why, what to test next. |

### Ingestion Pipeline — When a Document Arrives

```
DOCUMENT ARRIVES (any format)
          │
          ▼
┌─────────────────────────┐
│  intake.py              │  route by format → transcribe → chunk
│  writes L3 (immutable)  │  pptx / pdf / docx / audio / md
└─────────────────────────┘
          │
          ▼  RAW UNITS → L3 (once only, never rerun)
          │
          ▼
┌──────────────────────────────────────────┐
│  fact_extractor.py                        │
│  system: compiled fact_extractor_prompt   │
│  input:  L3 units + topics_touched        │
│  output: L2 fact log (per-stream)         │
│  captures signals at belief-claim level   │
│  surfaces distinct patterns separately    │
└──────────────────────────────────────────┘
          │
          ▼  FACT LOG → belief engine
          │
          ▼
┌──────────────────────────────────────────┐
│  belief_engine.py                         │
│  system: compiled belief_reasoning_prompt │
│  input:  existing belief.md (or NULL)     │
│          + fact log                       │
│  output: belief.md (evolved)              │
│          + belief_changelog.md (appended) │
│  volume check: ≥ 8 active beliefs         │
└──────────────────────────────────────────┘
```

---

## 05 — The Prompt Architecture

Three phases. Entity foundation runs once per entity. Stream setup runs once per stream. Document ingestion runs for every document.

### Phase 0 — Entity Foundation (runs once per entity)

| Prompt | What It Does |
|--------|-------------|
| **Prompt −1 — Entity Foundation** | Structured interview across five areas: business model and profitability thesis, thesis-defining metrics, normalization model, narration design, and what matters vs. noise. Produces `entities/{entity_id}/foundation.md`. Every stream for this entity reads it as its prior. |

### Phase 1 — Stream Setup (runs once per belief stream)

| Prompt | What It Does |
|--------|-------------|
| **Prompt 00 — Document Profile** | Interview agent. Given the entity foundation already exists, discovers document types, cadence, chosen angle, and what each document type can and cannot carry. Produces a structured Document Profile. |
| **Prompt 01 — Strategic Blueprint** | Reads the entity foundation and the Document Profile together. Produces the master configuration document in five sections: foundation reference, stream identity, signal matrix, belief definition (with claim-heading rule, 5-field format, volume check), and candidate belief seed set (8–15 grounded hypotheses). |
| **Prompt 03 — Belief Reasoning Compiler** | Reads the blueprint and compiles a self-contained runtime system prompt for the belief engine. Embeds the foundation context. Encodes the candidate seed set, claim-heading rule, 5-field format, volume check, no-renumber rule, and all evolution actions. |
| **Prompt 06 — Fact Extraction Compiler** | Reads the blueprint and compiled belief prompt and compiles the runtime system prompt for the fact extractor. Embeds foundation context (thesis metrics, normalization model, narration design). Mandates granularity to support 8–15 Candidate beliefs on the first document. One compiled extractor per document type. |

### Phase 2 — Document Ingestion (runs for every document)

| Component | What It Does |
|-----------|-------------|
| **intake.py** | Routes by format, transcribes, and splits into meaningful units. Writes immutable raw transcript to L3. |
| **fact_extractor.py** | Reads L3 units using the compiled fact extractor prompt. Extracts signals grounded in the foundation — at the granularity needed for belief-level claims, not summary-level observations. Writes fact log to L2. Does not interpret — only captures. |
| **belief_engine.py** | Reads the fact log and the existing belief memory using the compiled belief reasoning prompt. Numbered beliefs, claim-as-heading, 5-field format. Makes surgical updates to `belief.md`. Runs volume check (≥8 beliefs). Appends to `belief_changelog.md`. |

The cascade principle: quality injected at entity foundation and blueprint propagates forward without additional configuration. The belief engine at runtime receives only its compiled prompt, the existing `belief.md`, and the fact log. It never receives the foundation, the blueprint, or the raw document — everything it needs is already encoded inside the compiled prompt.

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

### What the Entity Foundation Agent (Prompt −1) Must Never Do

- Create beliefs. The foundation is the prior that beliefs are grounded in — it is not a belief and carries no confidence or direction.
- Describe documents. The foundation is about how the business works, not about how its documents are written. Document format belongs in the document profile and blueprint.
- Invent content. Only record what the user told you. If information is missing, say so explicitly and note what is unknown.
- Produce generic content. Every sentence must be specific enough that any reader immediately understands what this business cares about most — not what any business in its industry cares about.

### What the Document Profile (Prompt 00) Must Never Do

- Create beliefs. The interview only captures intent.
- Re-interview about the business. The entity foundation already exists and captures business model, metrics, and behavioral patterns. Prompt 00 focuses on documents and angle only.
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
- Write beliefs that fail the quality test: non-falsifiable, entity-generic, single-period, template artifacts, unsupported causality, not grounded in the foundation.
- Use category labels as headings. The heading of every belief entry is the claim itself — a complete, specific, falsifiable sentence. Not a topic name.
- Renumber beliefs. Once a belief is assigned #N, that number is permanent. Retired beliefs keep their number marked RETIRED.
- Let active belief count fall below 8 without auditing for umbrella beliefs and splitting them.
- Confuse RETIRE with CONTRADICT (perspective shift). RETIRE means the pattern ended — the behavior is no longer occurring. CONTRADICT with a perspective shift means the interpretive lens was wrong — the behavior was occurring but it meant something different than previously understood. When multiple consecutive TENSION signals accumulate, the engine must ask: did the pattern end (RETIRE), or did we misread what the pattern was (CONTRADICT + restatement)?

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

**01 — Belief entry field structure** *(resolved in v4)*
The belief entry now uses five fields: Statement, Why it matters, Evolution trail, Normal baseline, and Falsification test. The old concern about a separate "pattern fingerprint" field is resolved — the Evolution trail carries the concrete per-document evidence (specific language, structural positions, behavioral markers) as a first-person narrative of how the pattern developed. The Falsification test carries what would break it. No separate fingerprint field is needed.

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
