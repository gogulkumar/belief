# Shared Belief Doctrine

This doctrine defines what a belief means in this system. Every prompt that participates in the belief lifecycle must carry this understanding. Each prompt plays a different role, but all must share the same definition of what a belief is, what it is not, and how it is earned.

---

## The Foundation Layer

Before any belief stream begins, the entity foundation must exist. The foundation is not a belief and does not carry confidence or direction. It is the institutional understanding a strong analyst carries before opening any document: what kind of business this is, what its profitability thesis rests on, which metrics are thesis-defining, what normal looks like, and how this entity narrates its own performance.

Every belief in every stream for an entity is grounded in that entity's foundation. A belief that cannot be connected to the foundation — to the business model, the thesis metrics, or the narration design — is either too generic or based on a document artifact rather than how the business works.

The foundation lives at `entities/{entity_id}/foundation.md`. It is built once per entity, before any belief stream. It is a living document — refined as streams accumulate deeper understanding.

---

## What a Belief Is

A belief is not an opinion. Not a summary. Not a metric reading. Not a one-time observation. Not a forecast. Not a recommendation.

In this system, a belief is a **durable, falsifiable, and actionable interpretation** about how an entity behaves, communicates, explains performance, frames outcomes, or makes recurring decisions across comparable documents over time.

A belief is the kind of interpretation a strong senior analyst would carry in their head after reading many recurring documents from the same entity. It's not something they would abandon after reading one more document unless that document provides meaningful contradictory evidence.

**The master test: a belief must say something the document does not say.**

If you can find the belief by reading the document, it is an extraction, not a belief. The documents contain facts, signals, and patterns. Beliefs live between documents — in the judgment of someone who has read all of them and concluded something that none of them state explicitly.

The question to ask before writing any belief: *if a smart person who knows this sector but has never read this entity's documents reads this belief — do they learn something new and specifically important about how this business works that they couldn't have inferred from general knowledge? And does reading it change how they approach the next document?*

If the statement describes what happened, it is a fact. If it restates what the document says in different words, it is a summary. If it could apply to any company in this sector without modification, it is sector knowledge, not a belief. A belief is what a six-month reader sees that a first-time reader misses entirely — and what drives that distinction is interpretation, not observation.

**A belief can also be a discovered relationship** — a durable claim that one metric in this business drives another, with a stated direction, lag, and mechanism. These relationship beliefs are the connective tissue that turns isolated metric observations into a working model of the business. They are discovered from what the documents say, not pre-specified by the user. When a document states explicitly that A drives B, that is a relationship claim — the most structurally important signal the system can extract. It becomes a Candidate belief on the first document it appears in, and is confirmed or contradicted as more documents arrive.

A belief helps answer:

> "Before I open the next comparable document, what should I already expect to be true — and what would surprise me if it changed?"

That expectation is what makes the belief useful. For relationship beliefs, it also answers:

> "When this metric moves, what else should I expect to move — and when?"

---

## The Two-Test Gate

Every belief must pass two independent tests. Passing one does not excuse the other.

**Content Test — is it insightful?** This is the master test above: does the belief say something the document does not say? A belief that fails this is an extraction, a summary, or sector knowledge — no matter how carefully it was derived.

**Process Test — is it trustworthy?** Was the belief actually checked for being wrong, not just checked for being right? A pattern that was only ever confirmed — never actively searched for contradiction, never independently re-derived without the prior belief already in view — is unproven, no matter how many documents it has "held" across.

A belief can fail either test on its own:

- **Insightful but unproven** — a genuinely non-obvious claim that has only ever been confirmed by an extraction pass that already knew what belief it was supposed to find. Its apparent multi-document confirmation may be repetition of the same confirmation bias, not independent evidence.
- **Rigorously tested but trivial** — a belief with a clean audit trail — blind passes, active contradiction searches, clean provenance — that turns out to be something any competent analyst would already know without reading a single document from this entity.

Both tests are required before a belief is promoted past Candidate.

### Blind Pass vs. Belief-Aware Pass

The Process Test only means something if the system distinguishes these two extraction modes:

**Belief-aware pass** — the extractor is shown the existing belief before reading the new document, and asked whether the document confirms, contradicts, or is silent on it. This is the default mode for routine document processing.

**Blind pass** — the extractor reads the new document with no visibility into the existing belief, and independently reports whatever pattern it finds. This is the only way to prove a pattern is really there, rather than being an echo of what the extractor was told to look for.

A blind pass is required before a belief is promoted from Candidate to Provisional, and again before Provisional to Confirmed. If the blind pass at Provisional independently finds the same pattern the belief-aware pass reports as confirming, promote. If the blind pass finds nothing related while the belief-aware pass reports confirmation, do not promote — flag the belief for review instead; the confirmation may be an echo.

Once a belief reaches Established with a clean contradiction-search history, routine reconfirmation can run belief-aware only. Re-trigger a blind pass if something looks off — a silence, a near-miss, or a change to the foundation claim the belief depends on.

---

## The Claim Is the Heading

The heading of every belief entry is the claim itself — a complete, specific, falsifiable sentence. Not a category label. Not a topic name.

Wrong:
```
## Revenue Growth Character
```

Right:
```
## Belief #3 — Revenue growth is price-driven; volume has been in mild decline for three consecutive quarters.
```

The heading must carry enough specificity to be tested against the next document. A reader should be able to confirm or challenge the belief from the heading alone, before reading the body.

Beliefs are numbered once. Numbers are permanent. Retired beliefs keep their number marked RETIRED. The list is never renumbered.

---

## The Four Required Properties

Every belief must have four properties. Each is written below as a procedural test — not a descriptive claim anyone could assert with confident prose, but a check for what process must have actually happened before the property can be claimed.

### 1. Durable

Not: confirmed across N documents when the extractor saw the prior belief each time.
But: independently re-derived across N documents, with at least one extraction pass performed blind — with no visibility into the existing belief statement.

A belief must hold across multiple comparable documents, reporting periods, or recurring business reviews. A single document can suggest a possible belief, but one document alone cannot prove durability.

For example, if one business review says marketing efficiency improved because spend was reallocated across channels, that is not yet a belief. It is a candidate signal.

If several comparable business reviews repeatedly frame marketing efficiency as a controllable lever that explains downstream margin movement, then the system may begin to hold a belief about how this entity frames marketing efficiency — but only once at least one of those confirmations came from a pass that had no visibility into the belief it was confirming.

Durability means the pattern is not random, not one-period noise, and not merely a document summary. A belief confirmed only through belief-aware passes has not demonstrated durability — it has demonstrated that the extractor is consistent, which is a different property. Durability requires proof the pattern is really there, not just proof the system keeps agreeing with itself.

### 2. Falsifiable

Not: a falsification condition can be named in the entry.
But: the falsification condition has been actively searched for on every relevant document pass, with equal or greater priority than confirmation.

A belief must be capable of being proven wrong, narrowed, weakened, or retired by future documents.

If nothing could ever break the statement, it is not a belief. It is too vague.

A weak statement:

> The team cares about performance.

This is not falsifiable — every team cares about performance.

A stronger belief:

> The team consistently uses demand variance against plan as the first narrative anchor before explaining downstream revenue and margin movement.

This is falsifiable because a future document could stop leading with plan variance, shift the anchor to a different comparison, or explain margin independently of demand.

Naming a falsification test at creation means nothing if every subsequent pass only ever looked for confirming evidence. Contradiction-search must be an explicit, equally-weighted instruction on every pass — not a field filled in once and never revisited.

### 3. Grounded

Not: connects to the foundation at creation.
But: carries an explicit dependency on a named foundation claim, and is automatically flagged for re-review if that claim changes.

A belief that cannot be connected to the foundation — to the business model, the thesis metrics, or the narration design — is either too generic or based on a document artifact rather than how the business works. But "grounded in the foundation," checked once at creation and never revisited, means that if the foundation itself becomes outdated or wrong, every belief built on it stays wrong silently, with no mechanism to catch it.

Every belief entry names the specific foundation claim it depends on. If that claim is later revised, the belief is automatically flagged for re-review — not left standing on a foundation that no longer says what it said when the belief was created.

### 4. Actionable

Not: "would help an analyst read the next document."
But: a specific before/after answer to a stated example query can be produced, showing the belief changes the conclusion.

A belief must help an analyst read the next document better. It should create a useful expectation, baseline, or warning signal.

A belief should help the analyst know:

- what to look for
- what is normal
- what would be surprising
- what absence would matter
- what changed from prior behavior
- what deserves investigation

"Would help an analyst" is a narrative claim anyone can assert after the fact. The test requires naming one concrete example query and showing the answer actually differs with the belief present versus absent. If that before/after pair cannot be produced, the belief does not qualify, regardless of how well it reads.

---

## What a Belief Is Not

**A belief is not a metric reading.**
> Revenue reached $X in period Y.
This is a fact, not a belief.

**A belief is not a one-period observation.**
> The team cited external headwinds this period.
This may be evidence. It is not yet a belief.

**A belief is not a document summary.**
> The deck covers performance, pipeline, and outlook.
This describes the document. It does not create durable interpretation.

**A belief is not a management claim treated as truth.**
> Efficiency improved because of better process optimization.
If the document says this, it is reported attribution. The system may record that the team attributed the improvement to process optimization, but it must not treat that attribution as verified causality unless additional evidence supports it.

**A belief is not a forecast.**
> Performance will improve next period.
That is a prediction, not a belief.

**A belief is not a template artifact.**
> Measure A appears before Measure B in the deck.
This may only be true because the deck template forces that order. A stronger belief must explain the interpretive behavior inside the structure.

---

## The Dual Nature: Durable AND Provisional

A belief must be durable enough to act from. It should not be abandoned after one contradicting document. But a belief must also be provisional — open to revision when accumulated evidence warrants it.

These two properties aren't in tension. Durability without provisionality is bias. Provisionality without durability is noise. Together they define what makes a belief useful.

Nir Eyal in *Beyond Belief* frames this as practical and provisional: the best beliefs offer just enough certainty to act, yet enough flexibility to adapt. A belief that requires ignoring evidence to sustain itself is not a belief — it is a bias that will eventually blind the analyst to what the documents are actually showing.

### Two Kinds of Revision

Not all revision is the same. The system must distinguish between them.

**Incremental update** — The statement is refined as confidence rises or falls. The interpretive frame stays intact. A belief about how the entity attributes performance becomes more confident as more documents confirm it, or weakens as contradicting signals accumulate. Same lens, adjusted view.

**Perspective shift** — Enough evidence accumulates to suggest a fundamentally different interpretation of what has been observed. The interpretive frame itself changes. What looked like "cost efficiency behavior" was actually a response to a structural market constraint that has now resolved. The prior evidence trail does not become invalid — it is now understood differently under a new interpretation.

A perspective shift isn't a failure of the previous belief. The previous belief was the best available interpretation at the time, held provisionally. A perspective shift is what accumulated evidence is meant to produce: not just more confidence in the same frame, but occasionally, a different frame entirely.

### Beliefs Shape What You See Next

A belief is not merely a record of past patterns. It is an active filter on the next document. It determines what the analyst notices, what looks normal, and what looks like a signal worth investigating. This is why beliefs must be precise and falsifiable — a vague belief produces vague attention. A precise belief — with a named falsification test — produces directed attention.

---

## The Four Levels: Fact, Signal, Pattern, Belief

The system must distinguish these four levels and must not skip between them.

| Level | Definition | Example |
|-------|-----------|---------|
| **Fact** | Something directly stated or shown in a document | The review compares performance against plan and prior year |
| **Signal** | A fact that may matter for a belief stream | The review discusses plan variance before forecast variance |
| **Pattern** | A signal that appears repeatedly across comparable documents | Across multiple reviews, the team discusses plan variance first |
| **Belief** | The durable interpretation created from recurring patterns | This entity treats plan variance as the primary narrative anchor, while forecast is used as secondary calibration |

The system must not jump from fact to belief. It must move carefully: fact → signal → pattern → belief.

The fact log a signal or pattern was drawn from is not discarded once it has served that purpose. It is retained, permanently and addressably, as the memory a belief's Provenance record points back into. See "Fact Logs Are Memory, Not Scratch Paper" below.

---

## The Durability Ladder

Beliefs mature over time. The system must not treat all beliefs as equally strong — and must not promote a stage without the verification that stage requires.

| Stage | Documents | Meaning | Verification required to enter this stage |
|-------|-----------|---------|---------------------------------------------|
| **Candidate** | 1 | A signal with the shape of a durable pattern. Not yet a belief. Explicitly subject to confirmation. | None — nothing exists yet to check blind against. |
| **Provisional** | 2 | Two comparable documents support the pattern. Hold cautiously. | A blind pass on the second document must have independently found the same pattern. Belief-aware-only confirmation does not promote — flag for review instead. |
| **Confirmed** | 3 | Three comparable documents support the pattern. Treat as a baseline. | An active contradiction search on the third document must have found nothing. Confirmation without a contradiction search is an echo, not confirmation. |
| **Established** | 4+ | Four or more documents support the pattern. Breaking it is meaningful signal. | Holds across multiple document types if applicable. Still traces to a foundation claim that has not since changed. |
| **Retired / Contradicted** | — | A document broke the belief outright, its foundation claim was revised, or it went stale beyond the decay window. | Never deleted — kept and marked, so the system retains the memory that it was once believed and later proven wrong or allowed to expire. |

### Established Beliefs Decay Without New Evidence

An Established belief untouched by any relevant signal across N subsequent comparable documents downgrades to Confirmed automatically — not because it was proven wrong, but because "established and confirmed several documents ago" and "established and still being actively tested" are different confidence levels the system must be able to distinguish. Tag this action `[DECAY]` in the changelog — distinct from `[SILENCE]`, which notes an individual document's silence without consequence. `[DECAY]` fires once silence accumulates past the stream's configured threshold (system default: 4 consecutive silent comparable documents).

---

## Volume Check

A populated belief stream must hold at minimum 8 active beliefs. Fewer than 8 indicates over-collapse — umbrella statements that conflate multiple distinct patterns into one vague claim. When this happens, the belief engine must audit and split.

On the first document, the engine must initialize between 8 and 15 specific Candidate beliefs. Not 3–4 shallow umbrellas. Specific, falsifiable claims at the level of observable business behavior.

The purpose of the volume check is not to manufacture beliefs. It is to prevent the engine from producing beliefs that are too broad to be falsifiable — beliefs that cover so much ground they can never be proved wrong by a future document.

---

## Belief Entry Structure

Every belief entry must use this structure. The heading is the claim. The five narrative fields ground and test it. The Provenance record makes that testing checkable without parsing prose.

```markdown
## Belief #N — [The claim stated as a complete, falsifiable sentence.]   [ACTION_TAG]   Status: Candidate | Provisional | Confirmed | Established | Retired

**Statement**: The claim restated precisely and falsifiably, as business judgment grounded in how the business works (per the foundation). One sentence. Present tense. No hedging.

**Why it matters**: How this belief connects to the foundation — why it matters for how the business operates, which thesis metric it touches, what it reveals about business behavior.

**Evolution trail**: First-person, per-document journey. When first seen and in which document. What each subsequent comparable document added, confirmed, narrowed, or challenged. Current maturity and document count. Written as accumulating judgment, not a list of facts.

**Normal baseline**: What the next comparable document should show if this belief is holding, per the foundation's normalization model. On the first document: "not yet established — awaiting second comparable document."

**Falsification test**: What a future document must show to prove this wrong, narrow its scope, or retire it. Candidate: "fails to recur in the next comparable document." Mature beliefs: name a specific reversal.

**Provenance**:
- Foundation dependency: [the specific foundation claim this belief relies on]
- Confirming documents: [doc_ids]
- Blind passes: [doc_ids where this pattern was independently re-derived with no visibility into the prior belief — empty until one has run]
- Contradiction searches: [doc_ids checked, and what was found — "searched, none found" counts as a result; an absent line does not]
- Related beliefs: [belief IDs, in this stream or another, that share the same underlying phenomenon — empty if none identified yet]
- Last checked: [doc_id of the most recent document that produced any action on this belief, including SILENCE]
```

A well-written Evolution trail and an actually-verified belief must not be allowed to look identical. The Provenance record is the difference — it is what lets an agent citing a belief, or a person auditing one, determine trustworthiness without reading every sentence of the prose.

---

## Fact Logs Are Memory, Not Scratch Paper

Facts, memory, and beliefs are three different things, and the system must not collapse them:

- **Facts** are what a document literally states. A fact log captures them per document.
- **Memory** is the full, permanent, addressable record of every fact log ever produced — never compressed, never discarded once a belief has been drawn from it.
- **Beliefs** are the compressed, tested conclusions drawn from memory. Not memory itself — a claim about what memory, examined carefully and checked for contradiction, implies.

The per-document fact log is not disposable intermediate output consumed once and thrown away. It is the memory that belief entries point back into via their Provenance record's Confirming documents, Blind passes, and Contradiction searches lines. The Evolution trail is a readable narrative view generated from that memory — it does not compete with the Provenance record for truth, and it is not itself the record of what happened.

This distinction matters most when a belief turns out to be wrong. Marking a belief Retired or Contradicted tells you it failed. It does not tell you *why* the belief-formation process failed — whether the verification broke down, whether the foundation claim it depended on was wrong, or whether the sample was simply unlucky. That diagnosis is only possible if the memory a belief was built from is still there to trace back to.

---

## Domain-Specific Belief Guidance

In any belief stream, beliefs must be careful because documents often contain statements, measures, and framing choices that can look meaningful but may not be durable.

Each belief stream accumulates a different type of durable understanding. A strong belief belongs to exactly one stream — and the stream determines what form the belief takes.

**Stream 00 — Factual Understanding**

This stream holds what is verifiably true about how the business is measured — not what the numbers mean, but what they definitively are and whether definitions are consistent.

Strong beliefs look like:
- how a key metric is defined and whether that definition has held stable across documents
- what the benchmark sequence is and whether it ever shifts
- how a calculation basis differs from industry convention — and what that means for comparisons
- which data systems produce which metrics — and whether source consistency has held

The test: any analyst with the source document can verify this belief independently. If it requires interpretation, it belongs in a different stream.

**Stream 01 — Business Model Understanding**

This stream holds durable interpretations of how this specific business makes money — not what it reported, but what the underlying economic engine is.

Strong beliefs look like:
- what the profitability thesis is and whether it is holding
- whether growth is price-driven or volume-driven — and what that reveals about competitive position and ceiling risk
- where margin is structurally made and where it leaks
- whether the unit economics are improving or degrading across comparable periods

The test: this belief is true regardless of which specific period is being reviewed. If it depends on a particular quarter's result, it's a delivery observation (Stream 01 is about the model, not the track record).

**Stream 02 — Business Dynamics**

This stream holds how the parts of the business move relative to each other — lead-lag relationships, conversion chains, seasonal cycles, stress behavior, and feedback loops.

Strong beliefs look like:
- which metric moves first and which follows, with an established lag
- the conversion chain: from spend to output, step by step, with efficiency at each link
- what happens under pressure — which metric degrades first, which holds, which recovers last
- whether the system is self-correcting or self-amplifying

Relationship claims (explicit management statements that A drives B) can initialize a Candidate belief on the first document. Stress and feedback beliefs require observing at least one pressure episode.

**Stream 03 — Causal Understanding**

This stream holds what the team attributes as the causes of performance movement — and whether those attributions are honest, selective, or strategically chosen.

Strong beliefs look like:
- which causes appear first in the bridge and which appear last — the systematic sequencing of attribution
- whether external factors are consistently used to explain controllable misses
- what the gap is between stated attribution and what the dynamics stream shows actually moved
- whether the attribution pattern is consistent across comparable periods or shifts under different performance conditions

The test: a new analyst reading this belief would know what the team will say before opening the document — and would know whether to believe it. If the belief is about what actually caused performance (not what the team said caused it), it belongs in Stream 02.

**Stream 04 — Business Memory**

This stream holds how this organization behaves as a decision-making system under different conditions — what it actually does when performance is under pressure.

Strong beliefs look like:
- what gets protected and what gets cut, in what order, under pressure
- whether stated priorities survive contact with a difficult period
- what the organization has consistently treated as structurally untouchable regardless of conditions
- the behavioral pattern that repeats across multiple pressure episodes — not what happened once

The test: a new analyst reading this belief would know what the organization will do before it does it. If the belief is about what the document says about decisions (not the actual decision pattern evidenced across documents), it belongs in Stream 05.

**Stream 05 — Narrative Understanding**

This stream holds how this business tells its story — the deliberate communication choices that reveal what leadership wants you to think versus what the numbers actually show.

Strong beliefs look like:
- what the document leads with and what that choice signals about the underlying result
- how confidence language shifts under different performance conditions
- what topics disappear from the main document as they deteriorate
- specific words or phrases that recur with consistent meanings
- the gap between the framing and reality — what is being emphasized and what is being minimized

The test: a reader who had never opened this entity's document could predict the narrative structure before reading it. If the belief is about what actually happened (not how it was framed), it belongs in another stream.

**Stream 06 — Forecast and Plan Behavior**

This stream holds how this business plans, forecasts, and revises — the systematic behavioral patterns in how it sets and updates expectations.

Strong beliefs look like:
- whether Plan is set aggressively, conservatively, or realistically — and whether that varies by metric or period
- the direction, magnitude, and timing of forecast revisions across comparable periods
- systematic bias patterns — early-year optimism, late-year conservatism, or metric-specific tendencies
- whether stated risk and upside language materializes or is consistently wrong in the same direction

The test: a reader of this belief can predict, before the new forecast is released, approximately what it will say and in which direction it will be revised. If the belief is about what actual forecast vs actual values were in a specific period, it belongs in Stream 00.

---

**Across all streams:**

A belief isn't a raw reading of a single measure. It isn't a single result from one period. It isn't an unsupported causal claim. It isn't a generic statement that applies to any entity in any domain. It isn't a deck observation dressed as business insight.

**The belief must carry interpretation. The interpretation must say something the document does not say.**

---

## Belief Quality Test

Before checking the numbered questions below, apply the Two-Test Gate.

**Content Test:**

- Does this say something the document does not say explicitly?
- Would a smart analyst who knows this sector but has never seen this entity's documents understand something new and specifically important about this business by reading it?
- Does this reveal something that requires having read multiple documents — not inferrable from one?
- Does reading this change how you approach the next comparable document?

If the answer to any of these is no, the statement is a fact, a signal, or sector knowledge — not a belief. Don't write it.

**Process Test:**

- Has this pattern been independently re-derived by at least one blind pass — not just confirmed by a pass that already knew what belief it was checking?
- Has a contradiction search actually been run and reported, rather than assumed because nothing contradicting happened to surface?
- Does the Provenance record support the Status this belief is claiming, or does it show gaps that mean the status is aspirational?

If the answer to any of these is no, hold the belief at its current stage — or at Candidate if it has none of this evidence yet — regardless of how insightful the content is.

---

Before creating or updating a belief, also ask:

1. Is this more than a one-period fact?
2. Is this supported by document evidence?
3. Is this specific to the entity and belief stream?
4. Is this falsifiable by a future comparable document?
5. Does this help an analyst know what to expect next?
6. Is this free from unsupported causality?
7. Is this not merely a template artifact?
8. Does it preserve entity context — measure, period, comparison base — where relevant?
9. Does it identify what would confirm, weaken, or invalidate it?
10. Does it belong in a durable belief memory rather than a normal summary?
11. Is the heading a specific, testable claim — not a category label?
12. Is this grounded in the entity foundation — connected to the business thesis, not floating, with the specific foundation claim named?
13. Does the Provenance record match the Status being claimed — blind pass for Provisional and above, contradiction search for Confirmed and above?
14. Does this share an underlying phenomenon with any other belief in this or another stream? If so, is it cross-referenced under Related beliefs?

If the answer is no to any of these, don't create a belief. Record the item as a fact, signal, candidate, or unsupported interpretation instead.
