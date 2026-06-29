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

> **Question 1:** What do you want to do?
> - **Start from scratch** — new entity, no documents processed yet
> - **Process a new document** — beliefs already exist, new document arriving
> - **Use the belief memory** — answer a question, prepare for a meeting, or get a pre-read briefing
> - **Rebuild** — existing beliefs need to be regenerated
>
> **Question 2:** Do you have an entity foundation already built (`foundation.md`)?
> - **Yes** → Paste it in and I will read it before we start
> - **No** → I will build it first — it takes about 5 minutes

Based on the answers, take one of these paths:

| Situation | Path |
|-----------|------|
| No foundation, no beliefs | → Run Stage 1 (Foundation), then Stage 2 (Stream Setup), then Stage 3 (Compile), then Stage 4 (Process First Document) |
| Foundation exists, no beliefs | → Skip to Stage 2 (Stream Setup), then Stage 3 (Compile), then Stage 4 (Process First Document) |
| Foundation and beliefs exist, new document arriving | → Skip to Stage 5 (Update Existing Stream) |
| Foundation and beliefs exist, want to use the belief memory | → Skip to Stage 7 (Belief Activation) |
| Foundation and beliefs exist, want to rebuild | → Ask which rebuild option they want (see Stage 6) |

Tell the user which path you are taking before you begin.

---

## Stage 1 — Entity Foundation

**Skip this stage if the user already has a `foundation.md`.**

This is a short setup — 5 minutes. You are capturing entity identity and document context only. You are NOT asking the user to explain how the business works — the documents will teach the system that. Ask these questions one group at a time. Wait for each answer.

### Group 1 — Entity Identity
Ask:
- What is the name of this entity? (company, division, product line, team — whatever level this is)
- What kind of entity is it? One sentence: what does it do?
- Who produces the documents you will be reading? (management team, finance team, external research, etc.)

### Group 2 — Documents
Ask:
- What document types will flow through this system? (e.g., quarterly business review decks, earnings call transcripts, monthly reports, investor presentations)
- For each: how often do they arrive? What format are they in?
- How many documents do you have now? How many do you expect in the next six months?

### Group 3 — Any Known Context (Optional)
Ask:
- Is there anything specific about this entity that you already know and want the system to start from? (e.g., "I know this business is marketing-led" or "the key metric is NBV") — this is optional. If you say nothing, the system will discover the business model by reading the documents.
- Are there any known quirks in the documents that might mislead an AI reader? (e.g., "Q1 always looks weak due to seasonality" or "they use non-standard metric names")

Do NOT ask about the business model, operating chain, causal relationships, thesis metrics, normal ranges, or narration patterns. These will be discovered from documents. The user is not expected to know them upfront — that is precisely what the belief system is for.

**Output — foundation.md**

```
# [Entity Name] — Foundation

Built: [date]

## Entity Identity
Name: [Entity name]
Type: [What kind of entity — one sentence]
Document producer: [Who creates the documents this system will read]

## Documents
[For each document type:]
- Type: [Name]
- Cadence: [How often it arrives]
- Format: [PDF, PPTX, transcript, etc.]

## Known Prior Context
[Only what the user provided. If nothing: "None — business model and operating mechanics will be discovered from documents."]

## Known Document Quirks
[Only what the user provided. If nothing: "None identified."]

## Discovered Understanding
[Leave blank at setup. This section is populated as belief streams accumulate knowledge — Stream 02 relationship beliefs, Stream 01 delivery track record, etc. The business model lives here once the system has earned it from reading.]
```

**Success check — Stage 1:**
Show the foundation. Ask: Is the entity identity correct? Are the document types right? Anything you want to add to the known prior context?

Wait for confirmation. Correct anything flagged. Then say: "Foundation built. Moving to Stage 2 — Stream Setup."

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

Using the `fact_extractor_prompt.md` as your system context, read the document and produce a `fact_log.md`.

**First pass — Relationship Discovery:**
Before anything else, scan the entire document for explicit relationship claims — any statement where the document says that one metric, factor, or business component drives, causes, enables, or predicts another. These are the most valuable signals in the document because they reveal how the business works. Capture every one.

For each relationship claim:
```
## RELATIONSHIP CLAIM — [A] → [B]
Metric A: [Full name as used in document]
Metric B: [Full name as used in document]
Direction: [drives / precedes / enables / predicts / conditional]
Lag or timing: [If stated — exact language]
Condition: [If conditional — exact language]
Verbatim: "[Exact quote from document]"
Source position: [Where in the document this appeared]
```

If no relationship claims: output `NO RELATIONSHIP CLAIMS IN THIS DOCUMENT`

**Second pass — Named Candidate Signals:**
For each signal related to named candidates:
```
## SIGNAL — [Brief label]
Belief candidate: [Which candidate this feeds]
Signal type: [PRIMARY SIGNAL / PATTERN EVIDENCE / BASELINE READING / LEADING INDICATOR]
Content: [The extracted signal — verbatim quote, numeric with full context, or structural observation]
Source position: [Where in the document this appeared]
Comparison basis: [What benchmark or reference point, if applicable]
Absent signals: [What was expected but not present, if meaningful]
```

If no signal for a named candidate: output `CANDIDATE #N — NO SIGNAL IN THIS WINDOW`

**Success check — Step 6:**
Show the fact log, starting with any relationship claims found. Ask: Do these relationship claims accurately represent what the document stated? Did the extraction miss any explicit causal statements? Do the named candidate signals look accurate?

### Step 7 — Initialize Belief Memory

Using the `belief_reasoning_prompt.md` as your system context, read the fact log and initialize `belief.md`.

**Start with relationship claims.** For every relationship claim in the fact log, initialize a Stream 02 Candidate belief. A single explicit statement in a document is sufficient — the stated relationship is the evidence. Do not wait for multiple documents to confirm a relationship that the document already asserted directly.

For relationship beliefs:
```
## Belief #N — [Metric A] drives [Metric B] [with/in/within lag if stated]   [NEW_BELIEF]   Status: Candidate

**Statement**: [One falsifiable sentence: A drives B, with stated direction, lag, and mechanism if given]
**Why it matters**: [What this relationship means for how the business generates or loses value]
**Evolution trail**: First stated in [doc_id] — document explicitly said: "[verbatim]". Initialized as Candidate from explicit management statement. Not yet confirmed by observation across periods.
**Normal baseline**: Not yet established — will be set once the relationship has been observed playing out in a subsequent document.
**Falsification test**: A document where [A] moves but [B] does not follow within the stated lag, with no management explanation for the break, would challenge this relationship.
```

Then initialize all other Candidate beliefs from named candidate signals. Total first-document beliefs: 8–15 Candidates.

Every non-relationship belief entry:
```
## Belief #N — [The claim as a complete, falsifiable sentence]   [NEW_BELIEF]   Status: Candidate

**Statement**: [One falsifiable sentence, present tense, specific to this entity]
**Why it matters**: [Why this matters for how the business operates]
**Evolution trail**: First seen in [doc_id] — [brief observation of what this document showed]
**Normal baseline**: Not yet established — awaiting second comparable document.
**Falsification test**: [What a future document must show to break this]
```

Then produce `belief_changelog.md`:
```
## [doc_id] — [Date]

[For each new belief:]
[NEW_BELIEF] Belief #N — [claim] — [Relationship belief: initialized from explicit statement. Other: initialized from first observed signal.] Status: Candidate.
```

**Success check — Step 7:**
Show the belief memory. Ask:
- How many relationship claims from the document became Candidate beliefs? (If zero but the document discussed how the business works, the extraction may have missed statements)
- Are there at least 8 beliefs total? If fewer, audit for umbrella claims to split
- Are the relationship belief claims specific enough to be confirmed or broken — is the lag stated, the direction clear, the mechanism named?
- Does anything feel like a document summary rather than a durable pattern?

Wait for confirmation. Then say: "Belief memory initialized. Stream is live. When the next document arrives, run Stage 5. To use the belief memory now — to answer a question or prepare for a meeting — run Stage 7."

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

## Stage 7 — Belief Activation (Using the Belief Memory)

*Run this stage when beliefs already exist and the user wants to use them — not process a new document.*

Ask which use case:

> **What do you want to do with the belief memory?**
> - **Pre-read briefing** — I'm about to read the next document and want to know what to expect
> - **Answer a question** — I have a specific question about how this business works or what a metric means
> - **Meeting prep** — I'm about to go into a business review and want to know what to push on

### Use Case A — Pre-Read Briefing

Ask the user to paste in (or confirm) the current `belief.md`. Then produce:

```
## Pre-Read Briefing — [Entity] — [Next document: type and period]

### What to Expect
For each active belief with a Normal baseline:
[Belief #N — claim]: Expected: [what normal baseline says]. Deviation threshold: [what would be worth flagging].

### What Would Be a Signal
For each belief with a Falsification test that could trigger in this document:
[Belief #N — claim]: Watch for [what would challenge it]. If seen: [what to do].

### Relationship Chain Status
[List the established operating chain links from Stream 02 in order: A → B (lag X) → C (lag Y)...]
What this document should confirm or update: [which chain links this document type can carry evidence for]

### What to Ignore
[Beliefs that are SILENCE-likely for this document type — established patterns that this document type does not cover]
```

### Use Case B — Answer a Question

Ask the user their question. Then produce:

```
## Belief-Grounded Answer — [Question]

### Answer
[Direct answer in 1–2 sentences, calibrated by the belief evidence]

### What the Belief Memory Says
[For each relevant belief:]
Belief #N ([Status]): [What this belief says that is relevant. Quote the statement verbatim.]

### What Would Change This Answer
[What future evidence would revise the relevant beliefs — and therefore change the answer]

### What the Belief Memory Cannot Answer
[Any part of the question where no established belief exists — name the gap explicitly. If a Candidate belief exists but is not yet confirmed, say so.]
```

**Examples of questions this handles:**
- "Is Q1 S&M spend elevated or is this normal?" → Stream 02 ratio relationship belief answers
- "What drives revenue in this business?" → Stream 02 relationship beliefs answer
- "Should I be concerned about the revenue miss?" → Stream 01 delivery track record answers with base rate and context
- "Is this business self-correcting or does a miss compound?" → Stream 02 feedback dynamics belief answers (if established)
- "What language shift should make me worried?" → Stream 03 communication pattern beliefs answer
- "What is the normal booking range for Q2?" → Stream 01 or Stream 04 baseline answers

### Use Case C — Meeting Preparation

Ask the user to paste in (or confirm) the current `belief.md`. Then produce:

```
## Meeting Brief — [Entity] — [Meeting type, period]

### Three Things to Push On
[For each: the belief being tested, why it matters, the specific question to ask]
1. Push on [topic] — Belief #N says [X]. Ask: "[Specific question]." If the answer is [Y], it confirms. If [Z], flag for follow-up.
2. [Same structure]
3. [Same structure]

### Two Things to Accept Without Deep Investigation
[Patterns that are Established and expected — do not spend meeting time on what the belief memory predicts will be routine]
1. [Topic]: Belief #N says this is expected in this period. Accept unless deviation exceeds [threshold].
2. [Same structure]

### One Pattern to Watch
[The highest-uncertainty belief — Candidate or under TENSION — that this meeting might provide new signal for]
Belief #N ([Candidate/TENSION]): Watch for [specific signal]. If seen: [what it means].

### Relationship Chain Check
[Which chain links does this meeting's data allow you to test? For each: what confirms it, what challenges it]
```

**Success check — Stage 7:**
After producing any activation output, ask: Does this answer your question? Is there any part where the belief memory gave you less than you needed — either because the belief doesn't exist yet or because it's still Candidate?

If there are gaps, note them explicitly. Gaps in the belief memory are signals that more documents need to be processed, not that the system failed.

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
| 7 — Belief Activation | Pre-read briefing / Q&A answer / Meeting brief | Not stored — produced on demand |

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
