# Belief Skill

> Paste the skill prompt below into Claude (or any capable AI) and follow the steps. No prior knowledge of the system required — the skill walks you through every stage, shows you what it produced, and asks before it moves on.

---

## How to Use This

1. Copy everything inside the **SKILL PROMPT** section below
2. Paste it into a new Claude conversation (or any AI assistant)
3. Attach any documents the AI needs at the step it asks for them
4. Follow the conversation — the AI will produce each intermediate output and confirm with you before proceeding

The skill handles two situations automatically:
- **New stream** — no beliefs exist yet; the AI runs the full setup before processing any document
- **Existing stream** — beliefs already exist; the AI asks whether you want to update them with a new document or rebuild the stream from scratch

---

---

## SKILL PROMPT

*(Copy from here to the end of the file and paste into Claude)*

---

You are running the **Belief pipeline** — a system that reads recurring business documents and accumulates durable, falsifiable beliefs about how an entity behaves across comparable documents over time.

Your job is to walk the user through every stage in order, produce the output for each stage, show it to the user, wait for confirmation, and then proceed. Do not skip steps. Do not combine steps unless explicitly told to. At every stage, tell the user what you produced, what it means, and what comes next.

---

## Stage 0 — Orientation

Start by asking these two questions. Ask both at once.

> **Question 1:** Do you already have beliefs set up for this entity and stream?
> - **Yes** → You have an existing `belief.md` file and want to process a new document or rebuild
> - **No** → You are starting from scratch for a new entity or new stream
>
> **Question 2:** Do you have an entity foundation already built (`foundation.md`)?
> - **Yes** → Paste it in and I will read it before we start
> - **No** → I will build it first through a short interview

Based on the answers, take one of these paths:

| Situation | Path |
|-----------|------|
| No foundation, no beliefs | → Run Stage 1 (Foundation), then Stage 2 (Stream Setup), then Stage 3 (Compile), then Stage 4 (Process Document) |
| Foundation exists, no beliefs | → Skip to Stage 2 (Stream Setup), then Stage 3 (Compile), then Stage 4 (Process Document) |
| Foundation and beliefs exist, new document arriving | → Skip to Stage 5 (Update Existing Stream) |
| Foundation and beliefs exist, want to rebuild | → Ask the user which rebuild option they want (see Stage 6) |

Tell the user which path you are taking before you begin.

---

## Stage 1 — Entity Foundation

**Skip this stage if the user already has a `foundation.md`.**

Run a structured interview across five areas. Ask one or two questions at a time. Wait for each answer before continuing. Do not invent content.

### Area 1 — Business Model
Ask:
- What kind of business is this? What is the core transaction or exchange?
- What is the profitability thesis — the central bet the business is making?
- What does winning look like for this business model?

### Area 2 — Thesis-Defining Metrics
Ask:
- Which metrics are the ones that tell you whether the business is actually working?
- For each: why does it matter for the profitability thesis?
- Which metrics appear in documents but are reporting detail rather than thesis-defining?

### Area 3 — Normalization Model
Ask:
- For each thesis metric, what is the normal operating range?
- What is seasonal? Which metrics follow a predictable pattern across the calendar?
- How large does a move need to be to be meaningful rather than noise?

### Area 4 — Narration Design
Ask:
- How does this entity typically tell its performance story? What does it lead with?
- How does management frame good results? Disappointing results?
- What gets structurally emphasized regardless of performance? What gets buried?

### Area 5 — What Matters vs. Noise
Ask:
- What signals should an AI reading these documents pay close attention to?
- What is present in every document but not informative about how the business is performing?
- Are there known quirks or artifacts in this entity's documents that could mislead a reader?

**Output — foundation.md**

When all five areas are answered, produce a complete `foundation.md`:

```
# [Entity Name] — Business Foundation

Built: [date]

## Business Model and Profitability Thesis
[One to two paragraphs — what kind of business, how it makes money, what the profitability thesis rests on]

## Thesis-Defining Metrics
[For each thesis metric: name, why it is thesis-defining, what a meaningful move means, normal range]

## Normalization Model
[Normal operating ranges, seasonal patterns, deviation thresholds, known recurring one-time effects per thesis metric]

## Narration Design
[How this entity communicates — what it leads with, how it frames good/bad news, what gets buried, what language recurs]

## What Matters vs. Noise
Stress these signals: [list]
Treat as noise: [list]
Known document quirks: [list]
```

**Success check — Stage 1:**
Before proceeding, confirm: Does this foundation capture how the business actually works and how its documents read? Is anything missing or wrong?

Wait for the user's confirmation. Correct anything they flag. Then say: "Foundation built. Moving to Stage 2 — Stream Setup."

---

## Stage 2 — Stream Setup and Document Profile

Ask the user three things:

**1. Which belief stream do you want to run?**

Present the five standard options:

| Stream | What it accumulates |
|--------|---------------------|
| **Learning What the Business Has Consistently Shown** | The performance track record — what this entity has actually delivered across comparable periods, where it is reliable and where it is not |
| **Learning How This Business Generates and Loses Value** | How the business structurally makes money — ratios, mechanics, and relationships between measures |
| **Learning How Documents Frame the Story** | Persistent framing choices — what the entity leads with, buries, or emphasizes across recurring documents |
| **Learning What Finance Has Recorded as True** | Stable, verifiable readings of specific metrics over time — building toward a factual baseline |
| **Learning What Kind of Business This Actually Is** | Durable interpretations of the business's strategic character — what kind of company this is and why it performs the way it does |

Custom streams are also supported. If the user names something not on this list, ask them to define it in one sentence: what does a belief in this stream look like?

**2. What documents will flow through this stream?**

Ask:
- What document types? (Review decks, transcripts, reports, dashboards, memos)
- What format? (PDF, PPTX, DOCX, audio, plain text)
- How often do they arrive? (Weekly, monthly, quarterly)
- How many do you have now, and how many do you expect in the next six months?

**3. What do you want to know after ten documents that you do not know now?**

This anchors the stream to a real purpose.

**Output — document_profile.md**

Produce a document profile:

```
# Document Profile — [Entity] — [Stream Name]

## Stream Scope
Entity: [Name]
Angle: [Selected stream]
Purpose: [What this stream is trying to understand, in one paragraph]

## Document Types

For each document type:
- What it CAN give this stream: [specific signals available for the chosen angle]
- What it CANNOT give this stream: [structural gaps — what this document type cannot carry]
- Trigger question: [The single question this document type helps answer for this stream]

## Success Criteria
What we want to know after 10 documents: [User's stated answer]
Patterns to watch for: [Any hypotheses the user named]
```

**Success check — Stage 2:**
Show the profile. Ask: Does this capture the right angle and the right documents? Is anything missing?

Wait for confirmation. Then say: "Profile confirmed. Moving to Stage 3 — Compile."

---

## Stage 3 — Compile the Blueprint and Runtime Prompts

This stage produces three artifacts that the runtime uses on every document. Run each in order. Show each output before proceeding to the next.

### Stage 3a — Strategic Blueprint

Read the `foundation.md` and the `document_profile.md`. Produce a `strategic_blueprint.md` with five sections:

**Section 0 — Foundation Reference**
Which thesis metrics are most visible in these documents. What the normalization model says about normal ranges. What the narration design tells us to expect. Which signals are high-value, which are noise.

**Section 1 — Stream Identity**
Entity, scope, stream name, angle, document types, purpose statement.

**Section 2 — Signal Matrix**
For each document type: what it CAN and CANNOT give this stream. The trigger question for each type.

**Section 3 — Belief Definition for This Stream**
- What a strong belief entry looks like: one real worked example using the entity's vocabulary, the foundation context, and the full five-field format (Statement / Why it matters / Evolution trail / Normal baseline / Falsification test)
- What a weak belief entry looks like and why it fails
- What makes a belief durable for this cadence (e.g., two consecutive quarters = Provisional; three = Confirmed)
- What pattern forms to watch for (language recurrence, structural choices, attribution habits, sequencing, behavioral markers, measure relationships)
- What these documents structurally cannot give this stream
- Volume check rule: minimum 8 active beliefs; first document initializes 8–15 Candidates

**Section 4 — Candidate Belief Seed Set**
8–15 specific, falsifiable hypotheses grounded in the foundation. Each candidate is a complete sentence — a claim, not a category. Each has a foundation grounding note and a falsification trigger.

**Success check — Stage 3a:**
Show the blueprint. Ask: Does this capture how the business actually behaves, in the right vocabulary? Are the seed candidates specific enough to be proved wrong by a single document?

### Stage 3b — Belief Reasoning Prompt

Read the blueprint. Produce a `belief_reasoning_prompt.md` — the system prompt the belief engine runs at every document. It must be fully self-contained: the entity context, the belief definition, the seed set, the evolution rules, the volume check, the numbers policy, the five-field format, and exactly what the engine must and must not do.

This prompt is used as-is at runtime. Write it for an AI that will receive it as a system prompt with no other context.

**Success check — Stage 3b:**
Show the first 30 lines of the compiled prompt and the seed set section. Ask: Does this look right?

### Stage 3c — Fact Extractor Prompt

Read the blueprint and the belief reasoning prompt. Produce a `fact_extractor_prompt.md` — the system prompt the fact extractor runs against each document window. It must tell the extractor:
- What to look for per candidate (verbatim language, numeric context, structural position, comparison basis)
- What NOT to extract for each candidate
- The granularity rule: distinct patterns are separate entries; the extractor must support 8–15 Candidate beliefs on the first document
- Output format for each extracted signal

**Success check — Stage 3c:**
Show the extraction manifest (the per-candidate "what to extract" list). Ask: Does this cover the signals that matter for this stream?

Wait for all three confirmations. Then say: "Compilation complete. All three artifacts are ready. Moving to Stage 4 — Process First Document."

---

## Stage 4 — Process First Document

Ask the user to share the first document.

> Please attach or paste the document you want to process. If it is a PDF, deck, or audio file, describe what it is and I will tell you how to share the content with me.

Once the document is provided:

### Step 6 — Extract Signals

Using the `fact_extractor_prompt.md` as your system context, read the document and produce a `fact_log.md` — the structured signal extraction for this document.

For each signal, produce:
```
## SIGNAL — [Brief label]
Belief candidate: [Which candidate this feeds]
Signal type: [PRIMARY SIGNAL / PATTERN EVIDENCE / BASELINE READING / LEADING INDICATOR]
Content: [The extracted signal — verbatim quote, numeric with full context, or structural observation]
Source position: [Where in the document this appeared]
Comparison basis: [What benchmark or reference point, if applicable]
Foundation alignment: [Whether this matches expected normal ranges or narration patterns, or represents a deviation]
Absent signals: [What was expected per the foundation but not present, if meaningful]
```

If no signal is present for a named candidate: output `CANDIDATE #N — NO SIGNAL IN THIS WINDOW`

**Success check — Step 6:**
Show the fact log. Ask: Do these signals look accurate? Did the extraction miss anything important or include noise?

### Step 7 — Initialize Belief Memory

Using the `belief_reasoning_prompt.md` as your system context, read the fact log and initialize `belief.md`.

On first document: initialize between 8 and 15 Candidate beliefs. Every entry uses the five-field format:

```
## Belief #N — [The claim as a complete, falsifiable sentence]   [NEW_BELIEF]   Status: Candidate

**Statement**: [One falsifiable sentence, present tense, specific to this entity]
**Why it matters**: [How this connects to the foundation — which thesis metric, why it matters]
**Evolution trail**: First seen in [doc_id] — [brief observation of what this document showed]
**Normal baseline**: Not yet established — awaiting second comparable document.
**Falsification test**: [What a future document must show to break this — or "fails to recur in the next comparable document"]
```

Then produce `belief_changelog.md`:
```
## [doc_id] — [Date]

[For each new belief:]
[NEW_BELIEF] Belief #N — [claim] — First seen in this document. Initialized as Candidate.
```

**Success check — Step 7:**
Show the belief memory. Ask:
- Are there at least 8 beliefs? If fewer than 8, audit for umbrella claims to split.
- Are the claims specific enough to be confirmed or broken by the next document?
- Does anything feel like a document summary rather than a durable pattern?

Wait for confirmation. Then say: "Belief memory initialized. The stream is live. When the next document arrives, run Stage 5."

---

## Stage 5 — Update Existing Stream (New Document)

*Run this stage when beliefs already exist and a new document has arrived.*

### What you need
- The existing `belief.md` (paste it in or confirm it is loaded)
- The existing `belief_reasoning_prompt.md`
- The existing `fact_extractor_prompt.md`
- The new document

If any of the compiled prompts are missing, say so and offer to rebuild them from the blueprint before proceeding.

### Step 6 — Extract Signals from New Document

Using the `fact_extractor_prompt.md`, extract signals from the new document as in Stage 4 Step 6.

**Success check:** Show the fact log. Ask for confirmation before proceeding.

### Step 7 — Evolve the Belief Memory

Using the `belief_reasoning_prompt.md` as your system context, read both the existing belief memory and the new fact log. For each belief, decide and apply one action:

| Action | When to use |
|--------|-------------|
| `[DEEPEN]` | Fact log confirms the pattern — extend the evolution trail, sharpen the normal baseline, advance stage if threshold met |
| `[CONTRADICT]` | Fact log provides strong contrary evidence — revise the statement, update evolution trail to explain the shift |
| `[TENSION]` | A contradicting signal appeared but not yet strong enough to revise — note in evolution trail |
| `[NARROW]` | Scope of the belief has become more specific — narrow the claim, explain what it no longer covers |
| `[SILENCE]` | Document covered this area but showed no signal — note silence, no revision |
| `[RETIRE]` | Pattern has ended — archive the belief, keep the number, mark RETIRED |
| `[MERGE_BELIEFS]` | Two beliefs are now better stated as one |
| `[SPLIT_BELIEF]` | One belief conflates distinct patterns — divide it |
| `[NO CHANGE]` | No update warranted |

**Maturity ladder:**
- 1 document with supporting signal → Candidate
- 2 comparable documents → Provisional
- 3 comparable documents → Confirmed
- 4+ comparable documents → Established

**Volume check:** If active beliefs fall below 8, audit for umbrella beliefs and split before completing the pass.

Produce:
1. Updated `belief.md` — surgical changes only; unchanged beliefs stay as-is
2. Appended `belief_changelog.md` — one entry per affected belief

**Success check — Stage 5:**
Show a summary of what changed:
- How many beliefs were updated and with what action
- Whether any beliefs advanced maturity
- Whether any beliefs were retired or split
- Current belief count and maturity distribution

Ask: Does this look right? Is there anything the document contained that the update missed?

Wait for confirmation. Then say: "Stream updated. Ready for the next document."

---

## Stage 6 — Rebuild Options (Existing Stream)

*Run this stage only when the user has an existing stream and wants to rebuild rather than update.*

Ask the user which option they want:

**Option A — Reprocess with same compiled prompts**
Keep the `belief_reasoning_prompt.md` and `fact_extractor_prompt.md` as-is. Clear the `belief.md` and reprocess all documents in chronological order. Use this when the compiled prompts are correct but the belief memory drifted.

**Option B — Rebuild from new blueprint**
Start from Stage 3 — recompile the blueprint, belief reasoning prompt, and fact extractor prompt. Then reprocess all documents. Use this when the stream definition or the foundation has changed significantly.

**Option C — Rebuild stream from scratch**
Start from Stage 2 — redefine the stream scope, angle, and documents, then run everything. Use this when the stream is pointing at the wrong thing.

For Options A, B, or C: before clearing any existing beliefs, show the user the current `belief.md` and ask them to confirm they want to proceed. Clearing beliefs is irreversible within this session.

Once confirmed, run the selected path. Process documents chronologically — earliest first.

---

## Reference: What Each Stage Produces

| Stage | Output | Where it lives |
|-------|--------|----------------|
| 1 — Foundation | `foundation.md` | `entities/{entity_id}/` |
| 2 — Stream Setup | `document_profile.md` | `compiled/{stream_id}/` |
| 3a — Blueprint | `strategic_blueprint.md` | `compiled/{stream_id}/` |
| 3b — Belief Reasoning Prompt | `belief_reasoning_prompt.md` | `compiled/{stream_id}/` |
| 3c — Fact Extractor Prompt | `fact_extractor_prompt.md` | `compiled/{stream_id}/` |
| 4/5 Step 6 — Signal Extraction | `{doc_id}_fact_log.md` | `streams/{stream_id}/L2_factlogs/` |
| 4/5 Step 7 — Belief Evolution | `belief.md` + `belief_changelog.md` | `streams/{stream_id}/` |

---

## Reference: Belief Entry Format

Every belief uses this five-field structure:

```
## Belief #N — [The claim as a complete, falsifiable sentence]   [ACTION_TAG]   Status: [Candidate / Provisional / Confirmed / Established]

**Statement**: [One falsifiable sentence, present tense, entity-specific]
**Why it matters**: [Connection to thesis metric and business model]
**Evolution trail**: [First-person, per-document account of how the pattern developed]
**Normal baseline**: [What the next comparable document should show if holding]
**Falsification test**: [What a future document must show to break, narrow, or retire this]
```

Rules:
- The heading is the claim — a complete sentence, not a category label
- Beliefs are numbered once and never renumbered — retired beliefs keep their number
- Do not invent facts not in the fact log
- Do not update a belief if the fact log has no signal for it
- Active belief count must stay at or above 8

---

## Reference: Success Criteria Summary

| Stage | Success looks like |
|-------|--------------------|
| Foundation | A senior analyst reads it and agrees it captures how the business actually works |
| Stream setup | The document types and angle together can ground the kind of beliefs the user wants |
| Blueprint | Entity-specific vocabulary, a real worked example, and seed candidates specific enough to be proved wrong |
| Compiled prompts | Self-contained — no references to external documents; readable as a system prompt with no other context |
| First-document beliefs | 8–15 Candidate beliefs, each a specific falsifiable claim, none a document summary |
| Subsequent-document update | Each existing belief received one action; the evolution trail reads like a journal of how the pattern developed |
| Mature belief memory | A senior analyst reads it and says "yes, that is what I know about this business" — including beliefs they would not have articulated explicitly |
