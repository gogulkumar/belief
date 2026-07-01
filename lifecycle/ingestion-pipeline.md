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

**Who:** LLM, in two passes — a structured interview with the user at setup, and a corpus grounding pass once the first fact logs exist
**Input:** Pass 1: user interview across five areas (business model, thesis-defining metrics, normalization model, narration design, what matters vs. noise). Pass 2: all fact logs produced so far, read together (default trigger: 3 fact logs — `foundation_grounding_min_documents` in config)
**Output:** `entities/{entity_id}/foundation.md`

Produces the institutional business understanding that every belief stream for this entity reads as its prior. Not a document summary. Not a belief. The mental model a strong analyst carries before opening any document — what kind of business this is, what its profitability thesis rests on, which metrics are thesis-defining, what normal looks like, and how this entity narrates its own performance.

**Every claim is labeled with its source.** Interview claims are hypotheses (`source: interview`) — the user's stated understanding, useful but unverified. The corpus grounding pass reads the first N fact logs together and extracts what recurred *identically* across all of them — the definitional scaffold: metric definitions, benchmark sequences, decomposition structure. Those claims are `source: corpus`. Interview claims the corpus corroborates become `interview+corpus`; claims the corpus contradicts trigger an immediate Foundation Review; claims the corpus is silent on remain labeled hypotheses. The test separating a foundation claim from a belief: recurs identically and is definitional → foundation; could vary period to period and needs testing → belief.

Without the foundation, a belief stream produces document-level facts. With it, it produces grounded business judgment.

The foundation is built once per entity. All streams for that entity inherit it. The foundation is a living document — but it is only ever refined through the corpus grounding pass and the Foundation Review process (Step 7.5 below), never by silent edit. Every atomic claim in the output carries a stable claim ID (e.g. `foundation.business_model`) that belief Provenance records reference directly.

---

### Step 0 — Stream Setup (user-driven)

**Who:** The user, through a short questioning session
**Scope:** Capture three things: the entity, the requested belief stream (angle), and the document set that will feed it.
**Output:** A stream setup record: entity, belief stream name, document set

The belief stream may be one of the seven standard streams (00 — Factual Understanding, 01 — Business Model Understanding, 02 — Business Dynamics, 03 — Causal Understanding, 04 — Business Memory, 05 — Narrative Understanding, 06 — Forecast and Plan Behavior) or a custom stream (Marketing Efficiency Memory, Forecast Reliability, Operational Risk Memory, Investor Messaging, Margin Quality, Forecast Bias, or any user-defined stream). The user chooses. The documents determine what can be grounded.

This step only captures intent. The system grounds it in Steps 1 and 2.

---

### Step 1 — Document Profile (Prompt 00)

**Who:** LLM, once at setup — a short interview for what only the user knows, plus a **mandatory structural read** of one sample per document type
**Input:** requested belief stream, the user's answers (scope, document types, cadence, success criteria), one sample document per type, optional user purpose
**Output:** `compiled/{stream_id}/document_profile.md`, containing a **Structural Map** per document type

The division of knowledge: the user knows the business — that's the foundation. The documents are the only reliable witness to their own structure — users cannot recite their deck's anatomy, and asking them produces guesses. So Prompt 00 reads the samples and maps how each document tells its story: section inventory in order (verbatim titles), narrative assembly (what opens, what threads, what closes), how the pieces are stitched together, how the current period's story connects to the recurring structure, where numbers vs. commentary live, benchmarks as labeled, recurring apparatus, and observed absences.

The boundary: **map the telling, not the tale.** How the story is communicated, stitched, and connected — yes. What the story means — never. No performance judgment, no causal reading, no candidate beliefs, no cross-document pattern claims from a single sample. Does NOT extract signals — signal extraction is Step 6's job, against the compiled extractor prompt.

The CAN/CANNOT capability assessment is derived from the Structural Map, each line citing observed structure — never from type-level intuition. If no sample is available, the Structural Map is marked `UNGROUNDED — pending first document`, the CAN/CANNOT sections are explicitly provisional, and the first document processed at Step 6 must trigger completing the map and revisiting the profile before the compiled prompts are trusted.

The profile answers: what kind of durable beliefs could these documents eventually support if similar signals recur across comparable documents?

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
- **Section 3** — Belief definition for this stream (what a belief is here, claim-as-heading rule, entry structure (five narrative fields plus Provenance), durability test for this cadence, strong worked example, weak example, volume check rule — all in entity vocabulary)
- **Section 4** — Candidate belief seed set (8–15 specific candidate claims grounded in foundation knowledge; each is a complete falsifiable sentence about how this entity behaves)

Every section is answered with entity-specific content. No placeholders.

The angle is a hypothesis, not a constraint. If the documents cannot carry the requested stream, the blueprint says so and explains what CAN be extracted instead.

---

### Step 3 — Compile Belief Reasoning Prompt (Prompt 03)

**Who:** LLM compiler, once at setup, reading the blueprint
**Input:** `strategic_blueprint.md`
**Output:** `compiled/{stream_id}/belief_reasoning_prompt.md`

Compiles the blueprint into a self-contained runtime system prompt for the belief engine. At runtime the belief engine sees ONLY this prompt, the existing `belief.md` (or NULL), and the new fact log. So everything it needs must be inside: the belief doctrine, the entity-specific definition, the candidate belief seed set, the claim-heading rule, the entry format (five narrative fields plus Provenance), the numbers policy, the durability ladder with its promotion gating, the volume check, the first-document rule, the subsequent-document rule, the no-renumber rule, contradiction and silence handling, and all evolution actions.

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

## Phase 2 — Document Ingestion (Steps 5 – 8, including 7.5)

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

**Two extraction modes — belief-aware pass vs. blind pass:**

- **Belief-aware pass** (default): the existing belief is included in the input. The extractor is told what pattern it is checking for and reports confirm / contradict / silent.
- **Blind pass**: the existing belief is withheld from the input entirely. The extractor reads the document fresh and reports whatever pattern it independently finds, with no framing toward what it "should" see.

Run a blind pass whenever a belief is up for promotion — Candidate → Provisional, or Provisional → Confirmed (see Step 7). Outside of promotion decisions, a single belief-aware pass is sufficient for routine reconfirmation once a belief has reached Confirmed or Established with a clean verification history. Re-trigger a blind pass if something looks off: a silence, a near-miss, or a change to the foundation claim the belief depends on. This keeps cost bounded — full two-pass extraction is reserved for the moments where independent verification actually changes the outcome, not run on every document for every belief.

---

### Step 7 — Fact Log + Existing Belief → Evolved belief.md

**Who:** LLM, using `belief_reasoning_prompt.md` as system prompt
**Input:** `belief_reasoning_prompt.md` (system) + `{{EXISTING_DURABLE_BELIEF}}` (belief.md or NULL) + `{{NEW_CHRONOLOGICAL_FACT_LOG}}` (+ blind-pass fact log, when one was run)
**Output:** `streams/{stream_id}/belief.md` (evolved)

**On first document (belief.md is NULL):**
Initialize between 8 and 15 specific Candidate beliefs drawn from the fact log. Beliefs must use the candidate seed set from Blueprint Section 4 as a starting frame, but are grounded in what the first document actually showed. Every entry marked Status: Candidate. Evolution trail says "First seen in [doc_id] — [brief observation]." Normal baseline: "not yet established." Provenance record initialized with the foundation dependency named and all other fields empty — nothing exists yet to check blind against. No umbrella beliefs.

**Volume check:** If the active belief count falls below 8, audit for umbrella beliefs that conflate multiple distinct patterns and split them. Record the split in the changelog as [SPLIT_BELIEF].

**On subsequent documents:**
For each existing belief, decide: deepen / narrow / contradict / tension / silence / retire / no change. Update only when the evolution trail, baseline, or falsification test changes. Check contradiction and silence BEFORE deepening.

**Promotion gating — do not advance a stage without its required verification:**
- **Candidate → Provisional** requires a blind pass on this document that independently found the same pattern the belief-aware pass reports as confirming. If the blind pass found nothing related, do not promote — hold at Candidate and flag for review; the belief-aware confirmation may be an echo.
- **Provisional → Confirmed** requires an active contradiction search on this document, reported explicitly ("searched, none found") rather than assumed from silence. Confirmation without a reported contradiction search does not promote.
- **Confirmed → Established** requires the belief to hold across multiple document types where applicable, and its named foundation claim to be unchanged since the belief was created.
- **Established, decay:** if 4 consecutive comparable documents (configurable per stream) pass with no relevant signal for an Established belief, downgrade it to Confirmed. Tag `[DECAY]` — this is a Status change, distinct from `[SILENCE]`, which just notes an individual document's silence without consequence.

Every promotion, confirmation, and contradiction search updates the belief's **Provenance record** — Confirming documents, Blind passes, Contradiction searches, Last checked — with the current doc_id. The Evolution trail is written as a narrative account of what the Provenance record shows; the two must agree.

The belief engine does NOT invent evidence. Only holds what the fact log provides.

---

### Step 7.5 — Foundation Review Trigger Check

**Who:** Same engine pass as Step 7, or a lightweight follow-up check
**Input:** The just-updated `belief.md`, each affected belief's Provenance → Foundation dependency, and `entities/{entity_id}/foundation.md`
**Output:** Zero or more `[FOUNDATION_REVIEW]` changelog entries; if resolved this round, an updated `foundation.md` with an appended Foundation Revision Log entry

For every belief touched this round (any action in Step 7, not just DEEPEN), check: did this belief just reach Confirmed or Established status, and does its Statement add meaningful precision to, narrow, or contradict the foundation claim named in its own Provenance? A Candidate or Provisional belief never triggers this — the evidence has to have survived the Two-Test Gate first.

If the trigger fires, hand off to Prompt −1 running in **Foundation Review** mode (see `prompts/-1-generate-foundation.md`, "Foundation Review"). That pass surfaces the conflict between the recorded claim and the belief's independent finding, and the user resolves it as Adopt, Hold, or Defer.

- **Adopt**: `foundation.md`'s claim text is rewritten, a Foundation Revision Log entry is appended, and every other belief in every stream whose Provenance names that claim ID is flagged `[FOUNDATION_CHANGED]` in its own changelog — its Status is held, not retired, until its next document pass re-confirms grounding against the revised claim.
- **Hold**: no foundation change. The changelog records `[FOUNDATION_REVIEW]` with the resolution noted as Held and why.
- **Defer**: no foundation change. The changelog records `[FOUNDATION_REVIEW]` with the resolution noted as Deferred and what would resolve it.

This step never runs silently to a conclusion — Adopt vs. Hold is a decision surfaced to the user, not one the belief engine makes on its own, unless the stream's configuration explicitly sets automatic resolution.

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
| `[DECAY]` | Established belief downgraded to Confirmed after N consecutive silent comparable documents — a Status change, not a contradiction |
| `[FOUNDATION_REVIEW]` | A belief reaching Confirmed or Established triggered a Foundation Review; resolution (Adopt/Hold/Defer) recorded |
| `[FOUNDATION_CHANGED]` | A foundation claim this belief depends on was revised (Adopted) elsewhere; this belief is held pending re-grounding on its next document pass |
| `[NO CHANGE]` | Document processed; no update warranted for this belief |

The changelog is append-only. An absent changelog entry for a document means the pipeline did not complete for that document.

---

## The Runtime Contract

```
FOUNDATION (once per entity)
  Prompt -1 ← (user interview: business model, thesis metrics, normalization, narration, signals/noise)
           → entities/{entity_id}/foundation.md

SETUP (once per stream)
  Prompt 00 ← (requested_stream, user answers, ONE SAMPLE DOCUMENT
              PER TYPE — read in full for the Structural Map, purpose)
           → document_profile.md (Structural Map per document type)

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

**Blind-pass cost management:** running a blind pass alongside the belief-aware pass roughly doubles extraction cost for the documents where it's required. Reserve it for promotion decisions (Candidate → Provisional, Provisional → Confirmed) and periodic spot-checks on Established beliefs — not every document for every belief. At large document-set scale, batch documents into windows (e.g., quarters, if monthly cadence) rather than running the full pipeline as one long sequential chain: each window promotes what survives, and a process failure partway through is caught at the next window checkpoint rather than propagating silently through every remaining document.

---

## Failure Recovery

- **L3 is immutable:** if processing fails mid-pipeline, the raw extract is preserved. Reprocessing restarts from Step 6.
- **Fact log is permanent, not ephemeral:** it is the memory layer belief Provenance records point back into (see the doctrine's "Fact Logs Are Memory, Not Scratch Paper"). If an extraction pass fails before producing a complete fact log, re-run it against the preserved L3 units — but once a fact log is finalized and a belief cites it, it is retained indefinitely, addressable by doc_id.
- **belief.md is surgical:** a failed evolution pass does not corrupt existing beliefs. The pre-evolution state is unchanged until the write completes.
- **Changelog is append-only:** an absent changelog entry for a document means the pipeline did not complete for that document.
