# Ingestion Pipeline

How a belief stream is set up and how documents flow through it to produce belief updates.

---

## The Core Principle

Durable judgment is never created directly from a raw document. It is built only after profiling, blueprinting, extraction, and recurrence.

A single document can never manufacture a belief. Beliefs are earned across comparable documents over time.

---

## Two Phases

**Phase 1 — Stream Setup** runs once per belief stream, before any documents are processed.
**Phase 2 — Document Ingestion** runs every time a new document arrives.

---

## Phase 1 — Stream Setup (Steps −1 through 4)

### Step −1 — Entity Foundation (Prompt −1)

**Who:** LLM, running a structured interview with the user — once per entity, before any belief stream is created
**Input:** User interview across five areas: business model, thesis-defining metrics, normalization model, narration design, what matters vs. noise
**Output:** `entities/{entity_id}/foundation.md`

Produces the institutional business understanding that every belief stream for this entity reads as its prior. Not a document summary. Not a belief. The mental model a strong analyst carries before opening any document — what kind of business this is, what its profitability thesis rests on, which metrics are thesis-defining, what normal looks like, and how this entity narrates its own performance.

Without the foundation, a belief stream produces document-level facts. With it, it produces grounded business judgment.

The foundation is built once per entity. All streams for that entity inherit it. The foundation is a living document — it can be refined as streams accumulate deeper understanding.

---

### Step 0 — Stream Setup (user-driven)

**Who:** The user, through a short questioning session
**Scope:** Capture three things: the entity, the requested belief stream (angle), and the document set that will feed it.
**Output:** A stream setup record: entity, belief stream name, document set

The belief stream may be one of the five standard angles (Learning What the Business Has Consistently Shown, Learning How This Business Generates and Loses Value, Learning What Kind of Business This Actually Is, Learning How Documents Frame the Story, Learning What Finance Has Recorded as True) or a custom angle (Marketing Efficiency Memory, Forecast Reliability, Operational Risk Memory, Investor Messaging, Margin Quality, Forecast Bias, or any user-defined angle). The user chooses. The documents determine what can be grounded.

This step only captures intent. The system grounds it in Steps 1 and 2.

---

### Step 1 — Document Profile (Prompt 00)

**Who:** LLM, once at setup, against the first uploaded documents
**Input:** requested belief stream, uploaded documents, transcriptions, metadata, optional user purpose
**Output:** `compiled/{stream_id}/document_profile.md`

Profiles what these documents can ground for the requested stream. Identifies facts, signals, candidate patterns, and gaps. Does NOT create beliefs. Does NOT reject the stream.

The profile answers: what kind of durable beliefs could these documents eventually support if similar signals recur across comparable documents?

Strongest allowed language at this step: "candidate signal" or "possible pattern."

---

### Step 2 — Strategic Blueprint (Prompt 01)

**Who:** LLM, once at setup, reading the foundation and profile
**Input:** `entities/{entity_id}/foundation.md`, `document_profile.md`, requested belief stream, optional purpose and history
**Output:** `compiled/{stream_id}/strategic_blueprint.md`

Converts the profile into an answered configuration document, grounded in the foundation. Defines what a belief MEANS for this entity, this stream, these documents, this cadence.

The blueprint has five sections:
- **Section 0** — Foundation reference (how the entity foundation grounds this specific stream — which thesis metrics are relevant, which normalization model applies, what narration patterns are background knowledge)
- **Section 1** — Belief stream identity (entity, scope, exclusions)
- **Section 2** — Document types and signal matrix (what each document CAN and CANNOT carry, numbers policy, template artifact risk, trigger question)
- **Section 3** — Belief definition for this stream (what a belief is here, claim-as-heading rule, 5-field structure, durability test for this cadence, strong worked example, weak example, volume check rule — all in entity vocabulary)
- **Section 4** — Candidate belief seed set (8–15 specific candidate claims grounded in foundation knowledge; each is a complete falsifiable sentence about how this entity behaves)

Every section is answered with entity-specific content. No placeholders.

The angle is a hypothesis, not a constraint. If the documents cannot carry the requested stream, the blueprint says so and explains what CAN be extracted instead.

---

### Step 3 — Compile Belief Reasoning Prompt (Prompt 03)

**Who:** LLM compiler, once at setup, reading the blueprint
**Input:** `strategic_blueprint.md`
**Output:** `compiled/{stream_id}/belief_reasoning_prompt.md`

Compiles the blueprint into a self-contained runtime system prompt for the belief engine. At runtime the belief engine sees ONLY this prompt, the existing `belief.md` (or NULL), and the new fact log. So everything it needs must be inside: the belief doctrine, the entity-specific definition, the candidate belief seed set, the claim-heading rule, the 5-field format, the numbers policy, the durability ladder, the volume check, the first-document rule, the subsequent-document rule, the no-renumber rule, contradiction and silence handling, and all evolution actions.

Does NOT re-derive the belief definition from generic rules — reads Blueprint Section 3 directly.

---

### Step 4 — Compile Fact Extractor Prompt (Prompt 06)

**Who:** LLM compiler, once at setup, reading the blueprint and compiled belief prompt
**Input:** `strategic_blueprint.md`, `belief_reasoning_prompt.md`, document type context
**Output:** `compiled/{stream_id}/fact_extractor_prompt.md`

Compiles a belief-stream-scoped extraction prompt derived by working backward from what the belief engine needs. The extractor is grounded in the entity foundation — it knows which signals connect to thesis metrics and which are background noise per the foundation's what-matters-vs-noise section.

The extractor captures:
1. Signals that feed belief statements (specific, verbatim, falsifiable)
2. Evolution trail evidence (what was said, where, in what language — not paraphrased)
3. Normalization readings (reference the foundation's normal ranges)
4. Leading indicators for falsification tests

The extractor must surface distinct signals separately to support 8–15 Candidate beliefs on the first document. No collapsing distinct patterns into umbrella summaries.

Different document types in the same stream get different compiled extractor prompts. Each inherits from the same blueprint but has document-type-specific extraction guidance.

---

## Phase 2 — Document Ingestion (Steps 5 – 8)

Runs every time a new document arrives.

---

### Step 5 — Document Intake → L3 Units

**Who:** Code (intake.py)
**Input:** Uploaded document (pdf / pptx / docx / txt / audio)
**Output:** `streams/{stream_id}/L3_raw/{doc_id}.json`

Routes by format, transcribes, and splits into meaningful units. Each unit carries:
- `transcription` — full text content of this unit
- `topics_touched` — labels for what this unit covers
- `timestamp` — when processed
- `source_filename` — preserved from upload metadata

**Routing:**

| Document Type | Extraction Method |
|---------------|------------------|
| PPTX / image-heavy PDF | Vision model transcription (slide-by-slide) |
| Scanned PDF | OCR |
| Text PDF / DOCX / MD / TXT | Direct text extraction |
| Audio / video | Audio transcription → text |

The vision/OCR pass runs ONCE per document. The raw extract is stored permanently in L3 (Raw Archive). It is never reprocessed. One unit = one slide / section / page / transcript turn.

No interpretation. No extraction. Pure transcription and chunking.

---

### Step 6 — L3 Units → L2 Fact Log

**Who:** LLM, using `fact_extractor_prompt.md` as system prompt
**Input:** `fact_extractor_prompt.md` (system) + L3 units + topics + optional existing belief
**Output:** `streams/{stream_id}/L2_factlogs/{doc_id}_fact_log.md`

Extracts the evidence packet — angle-scoped, foundation-grounded, pattern-direction-aware. Organized to support individual belief claims, not watch areas.

The fact log is NOT a summary. It preserves for each extracted item its type: fact / metric / benchmark / attribution / relationship / pattern evidence / fingerprint candidate / silence / contradiction / template risk / out-of-scope.

Distinct signals are surfaced separately. If two different patterns appear in the same section of a document, they are two separate fact log entries — not one collapsed observation. This granularity is required to support 8–15 Candidate beliefs on the first document.

If the document exceeds the token window, it is processed in chunks. Each chunk produces fact-log output. Chunk outputs are compared against the running fact log to resolve overlap, redundancy, and drift before the document-level fact log is finalized.

The belief engine never receives the raw document. It reads the fact log.

---

### Step 7 — Fact Log + Existing Belief → Evolved belief.md

**Who:** LLM, using `belief_reasoning_prompt.md` as system prompt
**Input:** `belief_reasoning_prompt.md` (system) + `{{EXISTING_DURABLE_BELIEF}}` (belief.md or NULL) + `{{NEW_CHRONOLOGICAL_FACT_LOG}}`
**Output:** `streams/{stream_id}/belief.md` (evolved)

**On first document (belief.md is NULL):**
Initialize between 8 and 15 specific Candidate beliefs drawn from the fact log. Beliefs must use the candidate seed set from Blueprint Section 4 as a starting frame, but are grounded in what the first document actually showed. Every entry marked Status: Candidate. Evolution trail says "First seen in [doc_id] — [brief observation]." Normal baseline: "not yet established." No umbrella beliefs.

**Volume check:** If the active belief count falls below 8, audit for umbrella beliefs that conflate multiple distinct patterns and split them. Record the split in the changelog as [SPLIT_BELIEF].

**On subsequent documents:**
For each existing belief, decide: deepen / narrow / contradict / tension / silence / retire / no change. Update only when the evolution trail, baseline, or falsification test changes. Check contradiction and silence BEFORE deepening.

The belief engine does NOT invent evidence. Only holds what the fact log provides.

---

### Step 8 — Changelog

**Who:** Same engine pass as Step 7, second output
**Input:** Same as Step 7
**Output:** `streams/{stream_id}/belief_changelog.md` (appended)

Records what changed and why — the audit trail.

For each belief affected, records:
- Action (see action tags below)
- Previous statement (if changed)
- New statement (if changed)
- Reason
- Maturity impact
- What the next comparable document should test

**Changelog action tags:**

| Tag | Meaning |
|-----|---------|
| `[NEW_BELIEF]` | A belief entered for the first time (Status: Candidate) |
| `[DEEPEN]` | Belief held; evolution trail extended with new confirming evidence |
| `[NARROW]` | Belief scope reduced — applies more specifically than before |
| `[CONTRADICT]` | Strong contradicting evidence; belief statement revised |
| `[TENSION]` | A contradicting signal appeared but not yet strong enough to revise |
| `[SILENCE]` | The document covered this belief area but showed no signal |
| `[RETIRE]` | Belief archived — pattern ended; number kept, marked RETIRED |
| `[MERGE_BELIEFS]` | Two beliefs collapsed into one more precise claim |
| `[SPLIT_BELIEF]` | One belief divided into two distinct, separately falsifiable claims |
| `[NO CHANGE]` | Document processed; no update warranted for this belief |

The changelog is append-only. An absent changelog entry for a document means the pipeline did not complete for that document.

---

## The Runtime Contract

```
FOUNDATION (once per entity)
  Prompt -1 ← (user interview: business model, thesis metrics, normalization, narration, signals/noise)
           → entities/{entity_id}/foundation.md

SETUP (once per stream)
  Prompt 00 ← (requested_stream, documents, transcriptions, metadata, purpose)
           → document_profile.md

  Prompt 01 ← (foundation.md, document_profile.md, requested_stream, purpose, history)
           → strategic_blueprint.md

  Prompt 03 ← (strategic_blueprint.md)
           → belief_reasoning_prompt.md

  Prompt 06 ← (strategic_blueprint.md, belief_reasoning_prompt.md, doc_type_context)
           → fact_extractor_prompt.md

RUNTIME (per document)
  intake.py         ← uploaded_document
                    → L3_raw/{doc_id}.json

  fact_extractor.py ← fact_extractor_prompt.md (system)
                    + L3 units + topics_touched + optional existing belief
                    → L2_factlogs/{doc_id}_fact_log.md

  belief_engine.py  ← belief_reasoning_prompt.md (system)
                    + {{EXISTING_DURABLE_BELIEF}} = belief.md or NULL
                    + {{NEW_CHRONOLOGICAL_FACT_LOG}} = fact log
                    → belief.md (rewritten) + belief_changelog.md (appended)
```

The belief engine NEVER receives the raw document or the blueprint at runtime. Everything it needs is inside the compiled `belief_reasoning_prompt.md` and the fact log. This is the contract that makes the system reproducible and auditable.

---

## Token Budget

The fact extraction pass (Step 6) and belief evolution pass (Step 7) are the most expensive steps. For a large document:

- Chunks: configurable (default 50,000 tokens per chunk)
- Each chunk: one fact extraction call
- Fact log: one belief evolution call per stream

The compiled prompts are stream-specific and run separately per stream. Multiple streams processing the same document each get their own extracted fact log.

---

## Failure Recovery

- **L3 is immutable:** if processing fails mid-pipeline, the raw extract is preserved. Reprocessing restarts from Step 6.
- **Fact log is ephemeral:** if the extraction pass fails, it can be re-run against the preserved L3 units.
- **belief.md is surgical:** a failed evolution pass does not corrupt existing beliefs. The pre-evolution state is unchanged until the write completes.
- **Changelog is append-only:** an absent changelog entry for a document means the pipeline did not complete for that document.
