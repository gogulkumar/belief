# Belief Memory Schema

The belief memory (L1) is the living belief memory for one belief stream. It is the file an agent loads at session start. It is surgically updated — never rewritten from scratch.

## File Structure

One belief memory file per active belief stream. Each stream lives under its own directory alongside its raw archive and fact logs. Every stream for an entity reads the entity's `foundation.md` as its prior.

```
entities/{entity_id}/
└── foundation.md                ← entity-level business understanding (built once, before any stream)

compiled/{stream_id}/
├── document_profile.md
├── strategic_blueprint.md
├── belief_reasoning_prompt.md
└── fact_extractor_prompt.md

streams/{stream_id}/
├── belief.md                    ← the belief memory (L1)
├── belief_changelog.md          ← append-only audit trail
├── L2_factlogs/
│   └── {doc_id}_fact_log.md    ← per-document extracted signals
└── L3_raw/
    └── {doc_id}.json            ← immutable transcription archive
```

---

## Belief Entry Format

Each belief is one numbered block inside `belief.md`. The heading is the claim itself — a complete, specific sentence. Beliefs are numbered once and never renumbered. Retired beliefs keep their number marked RETIRED.

```markdown
## Belief #N — [The claim stated as a complete, falsifiable sentence.]   [ACTION_TAG]   Status: Candidate | Provisional | Confirmed | Established | Retired

**Statement**: The claim restated precisely and falsifiably, as business judgment grounded in how the business works (per the foundation). One sentence. Present tense. No hedging.

**Why it matters**: How this belief connects to the foundation — why it matters for how the business operates, which thesis metric it touches, what it reveals about business behavior that is not obvious from any single document.

**Evolution trail**: First-person, per-document journey. When it was first seen and in which document. What each subsequent comparable document added, confirmed, narrowed, or challenged. What the current maturity is and how many documents it rests on. Written as accumulating judgment, not a list of facts.

**Normal baseline**: What the next comparable document should show if this belief is holding, expressed in terms of the foundation's normalization model. On the first document: "not yet established — awaiting second comparable document."

**Falsification test**: What a future document must show to prove this wrong, narrow its scope, or retire it. For Candidate stage: "fails to recur in the next comparable document." For mature beliefs: name a specific reversal — a concrete signal that would force revision.
```

---

## Durability Ladder

The **Status** field tracks how many comparable documents have supported the pattern. Do not skip stages.

| Stage | Documents | Meaning |
|-------|-----------|---------|
| **Candidate** | 1 | A signal with the shape of a durable pattern. Not yet a belief. Explicitly subject to confirmation. Every first-document entry is Candidate. |
| **Provisional** | 2 | Two comparable documents support the pattern. Hold cautiously. |
| **Confirmed** | 3 | Three comparable documents support the pattern. Treat as a baseline. |
| **Established** | 4+ | Four or more documents support the pattern. Breaking it is meaningful signal, not noise. |

A Candidate entry is subject to confirmation by the next comparable document. An Established belief requires substantially stronger contradicting evidence before revision.

---

## Volume Check

A populated belief stream must hold at minimum 8 active beliefs. Fewer than 8 indicates over-collapse — beliefs that are umbrella statements covering multiple distinct patterns. The belief engine must self-audit when the count falls below 8 and split any umbrella beliefs into their constituent claims.

On the first document, the belief engine must initialize between 8 and 15 specific Candidate beliefs. Not 3–4 shallow umbrellas. Specific, falsifiable claims at the level of observable business behavior.

---

## Example

```markdown
## Belief #1 — The business leads every quarterly review with volume growth before any cost discussion.   [DEEPEN]   Status: Confirmed

**Statement**: Every quarterly review opens with volume performance against prior year before addressing any cost or margin line — establishing volume as the primary narrative frame through which all other performance is interpreted.

**Why it matters**: The foundation identifies volume growth as the primary thesis metric. Leading with volume signals that management reads its own performance through this lens first — cost and margin are downstream of the volume story, not parallel to it. This shapes how to interpret any period where volume leads strong but margins miss.

**Evolution trail**: First seen in Q4 2025 review — volume metrics occupied slides 2–3, with the first cost discussion not appearing until slide 7. I treated this as a possible structure, not yet a pattern. Q1 2026 repeated the same ordering: two volume slides, then pipeline, then cost efficiency. The structure held even though volume growth had slowed. By Q2 2026, with three consecutive reviews following this sequence, I treat this as how this business reads itself. The volume-first ordering is not driven by which metric performed better — it held in Q1 when volume was weak.

**Normal baseline**: Next comparable document should open with volume performance on slides 2–4, with cost/margin content not appearing until at least slide 6. If the ordering inverts, note it as a signal worth watching.

**Falsification test**: Two consecutive quarterly reviews that lead with margin or cost performance before volume would break this belief. A single inversion is worth noting as TENSION but not yet sufficient to revise.
```

---

## Changelog Structure

After every document, `belief_changelog.md` records what changed — one entry per affected belief, appended.

```markdown
## Changelog — {doc_id} — {timestamp}

### Belief #1 — The business leads every quarterly review with volume growth before any cost discussion.
**Action:** [DEEPEN]
**Previous statement:** (unchanged)
**New statement:** (unchanged)
**Reason:** Third consecutive document confirms volume-first ordering. Evolution trail extended with Q2 2026 observation that ordering held even when volume growth decelerated.
**Maturity impact:** Provisional → Confirmed
**What next document should test:** Whether volume-first ordering holds in a period where volume is negative or flat.

### Belief #4 — Management attributes margin misses exclusively to external cost inflation.
**Action:** [TENSION]
**Previous statement:** Management attributes margin misses exclusively to external cost inflation.
**New statement:** (unchanged — held under tension)
**Reason:** Q2 document included one reference to internal procurement inefficiency contributing to cost pressure — first internal attribution observed. Single document; not yet sufficient to revise.
**Maturity impact:** None — held at Confirmed pending next document
**What next document should test:** Whether internal attribution recurs or Q2 reference was isolated.
```

### Changelog Action Tags

| Tag | Meaning |
|-----|---------|
| `[NEW_BELIEF]` | A belief entered for the first time (Status: Candidate) |
| `[DEEPEN]` | Belief held; evolution trail extended with new confirming evidence |
| `[NARROW]` | Belief scope reduced — the claim now applies more specifically than before |
| `[CONTRADICT]` | Strong contradicting evidence; belief statement revised under a new interpretation |
| `[TENSION]` | A contradicting signal appeared but is not yet strong enough to revise the belief. Multiple consecutive TENSION entries signal a revision may be approaching. |
| `[SILENCE]` | The document covered this belief area but showed no signal — noted but no update |
| `[RETIRE]` | Belief archived — the pattern ended. The number is kept; the belief marked RETIRED. |
| `[MERGE_BELIEFS]` | Two beliefs collapsed into one more precise claim |
| `[SPLIT_BELIEF]` | One belief divided into two distinct, separately falsifiable claims |
| `[NO CHANGE]` | Document processed; no update warranted for this belief |

The changelog is append-only. An absent changelog entry for a document means the pipeline did not complete for that document.

---

## Update Arithmetic

| Action | Confidence Change |
|--------|------------------|
| Confirm (DEEPEN) | +0.08 |
| Contradict (TENSION) | −0.15 |
| New Prior (NEW_BELIEF) | initialized at 0.20 |
| Decay (90+ days unseen) | −0.05 per cycle |

**Bounds:** Floor 0.05 · Cap 0.95 (0.90 for causal beliefs)
**Archive threshold:** Below 0.10 → moved to archive; not deleted

---

## What the Belief Engine Reads at Runtime

The belief engine receives exactly three inputs at runtime. It receives nothing else.

1. **The compiled belief reasoning prompt** — `compiled/{stream_id}/belief_reasoning_prompt.md` — the self-contained system prompt encoding the doctrine, angle definition, candidate belief seed set, evolution rules, and worked examples for this stream. Produced once at setup from the Strategic Blueprint. Never changes unless the stream is reconfigured.
2. **The existing belief memory** — `streams/{stream_id}/belief.md` — the current state of all numbered beliefs in this stream. May be NULL for the first document.
3. **The fact log** — `streams/{stream_id}/L2_factlogs/{doc_id}_fact_log.md` — signals extracted from the current document, organized by belief area.

The belief engine never receives the raw document, the blueprint, or any other context. Everything it needs is inside the compiled prompt and the fact log. This is the contract that makes the system reproducible and auditable.

---

## Belief Memory Bounds

- **Min active beliefs per stream:** 8 (enforced by volume check)
- **Max active beliefs per stream:** ~20 (configurable)
- **Decay trigger:** 90 days since last confirming document (configurable)
- **Archive, not delete:** Contradicted and decayed beliefs are archived, not erased. History is preserved.
- **No renumbering:** Once a belief is assigned #N, that number is permanent. Retired beliefs keep their number marked RETIRED.
