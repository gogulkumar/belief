# World Model Schema

The world model (L1) is the living belief memory for one belief stream. It is the file an agent loads at session start. It is surgically updated — never rewritten from scratch.

## File Structure

One world model file per active belief stream. Each stream lives under its own directory alongside its raw archive and fact logs.

```
compiled/{stream_id}/
├── document_profile.md
├── strategic_blueprint.md
├── belief_reasoning_prompt.md
└── fact_extractor_prompt.md

streams/{stream_id}/
├── belief.md                    ← the world model (L1)
├── belief_changelog.md          ← append-only audit trail
├── L2_factlogs/
│   └── {doc_id}_fact_log.md    ← per-document extracted signals
└── L3_raw/
    └── {doc_id}.json            ← immutable transcription archive
```

---

## Belief Entry Format

Each belief is one block inside `belief.md`. Fields are always present; values update over time.

```markdown
## [WATCH_AREA_NAME]

**Statement:** [Durable, falsifiable, actionable interpretation of how the entity behaves]

**Validity scope:** [Entity, document type, cadence, known exclusions]

**Confidence:** 0.XX
**Direction:** Improving | Stable | Deteriorating | Unclear
**Stage:** Candidate | Provisional | Confirmed | Established
**Last Seen:** [Source filename of most recent confirming document]
**First Seen:** [Source filename of originating document]
**Updated from:** [Cumulative list of source filenames, in order, across all documents processed]

**Why durable:** [What repeated evidence supports this. What would break it. How many documents would confirm it.]

**Pattern fingerprint:** [Concrete recurring evidence — specific language, structural positions, sequencing behavior, measure relationships, attribution habits. This is what makes the belief auditable and testable across documents.]

**Normal baseline:** [The expected or typical state for this watch area — the reference point that makes deviations meaningful. Not what happened — what is normal.]

**Forward signal:** [What the next comparable document should show if this belief is holding. What signal would indicate it is shifting or breaking.]

**Invalidation signal:** [What would force revision, suspension, narrowing, or retirement of this belief.]

**Evidence trail:**
- [{doc_id}] — [brief note on what it showed]
- [{doc_id}] — [brief note on what it showed]

**Contradictions:** (if any)
- [{doc_id}] — [brief note on what challenged this belief]
```

---

## Durability Ladder

The **Stage** field tracks how many comparable documents have supported the pattern. Do not skip stages.

| Stage | Documents | Meaning |
|-------|-----------|---------|
| **Candidate** | 1 | A signal with the shape of a durable pattern. Not yet a belief. Every first-document entry is Candidate. |
| **Provisional** | 2 | Two comparable documents support the pattern. Hold cautiously. |
| **Confirmed** | 3 | Three comparable documents support the pattern. Treat as a baseline. |
| **Established** | 4+ | Four or more documents support the pattern. Breaking it is meaningful signal, not noise. |

A Candidate entry is explicitly marked as subject to confirmation. An Established belief requires substantially stronger contradicting evidence before revision.

---

## Example

```markdown
## Revenue Growth Character

**Statement:** Revenue growth is price-driven with volume in mild decline, monetizing an existing base rather than expanding it.

**Validity scope:** Entity-wide, business review documents, quarterly cadence. Excludes new market launches (structurally different dynamics).

**Confidence:** 0.52
**Direction:** Deteriorating
**Stage:** Confirmed
**Last Seen:** Business_Review_Q2_2026.pptx
**First Seen:** Business_Review_Q4_2025.pptx
**Updated from:** Business_Review_Q4_2025.pptx, Business_Review_Q1_2026.pptx, Business_Review_Q2_2026.pptx

**Why durable:** Observed in three consecutive quarterly reviews. Volume negative in all three; price positive in all three. Would break if volume returned positive for two consecutive quarters.

**Pattern fingerprint:** Volume metric appears immediately after revenue headline in every document. Price-volume split labeled explicitly in the performance section. Language: "pricing leverage" used in Q4 and Q1; "monetization efficiency" in Q2 — semantic drift but structural consistency. Volume decline named last in attribution, minimized relative to price contribution.

**Normal baseline:** Revenue growth 7–11%, composed of +8–12% price offset by −1–3% volume. Price carries the headline.

**Forward signal:** Next document should show price contribution still positive and volume still negative. If volume contribution turns positive, re-examine belief — single document would move to Tension, two consecutive positive volume quarters would force revision.

**Invalidation signal:** Two consecutive quarters of positive volume growth, or a management acknowledgment that the business is actively volume-expanding.

**Evidence trail:**
- [Business_Review_Q4_2025.pptx] — Revenue +9% on +11% price, volume −1%. First signal.
- [Business_Review_Q1_2026.pptx] — Pattern repeats. Volume −2%, price +8%. Confirmed.
- [Business_Review_Q2_2026.pptx] — Volume now −3%, price holding +7%. Direction tightening.

**Contradictions:**
(none)
```

---

## Causal Belief Entry Format

Causal beliefs carry additional fields for the lead-lag relationship.

```markdown
## [CAUSAL_WATCH_AREA]

**Leading signal:** [what moves first, with threshold if visible]
**Lagging outcome:** [what follows]
**Observed lag:** ~N comparable documents
**Confirmed:** N times
**Status:** Candidate | Provisional | Confirmed | Established

**Confidence:** 0.XX  (cap: 0.90)
**Direction:** Improving | Stable | Deteriorating | Unclear

**Evidence trail:**
- [{doc_id}] — leading signal appeared
- [{doc_id}] — lagging outcome arrived N periods later

**Contradictions:** (if any)
```

The confidence cap for causal beliefs is 0.90. Causal relationships are probabilistic, not deterministic. Confidence can approach but never fully assert mechanical certainty.

---

## Update Arithmetic

| Action | Confidence Change |
|--------|------------------|
| Confirm | +0.08 |
| Contradict | −0.15 |
| New Prior | initialized at 0.20 |
| Decay (90+ days unseen) | −0.05 per cycle |

**Bounds:** Floor 0.05 · Cap 0.95 (0.90 for causal beliefs)
**Archive threshold:** Below 0.10 → moved to archive; not deleted

---

## What the Belief Engine Reads at Runtime

The belief engine receives exactly three inputs at runtime. It receives nothing else.

1. **The compiled belief reasoning prompt** — `compiled/{stream_id}/belief_reasoning_prompt.md` — the self-contained system prompt encoding the doctrine, angle definition, watch areas, evolution rules, and worked examples for this stream. Produced once at setup from the Strategic Blueprint. Never changes unless the stream is reconfigured.
2. **The existing world model** — `streams/{stream_id}/belief.md` — the current state of all beliefs in this stream. May be NULL for the first document.
3. **The fact log** — `streams/{stream_id}/L2_factlogs/{doc_id}_fact_log.md` — signals extracted from the current document, organized by watch area.

The belief engine never receives the raw document, the blueprint, or any other context. Everything it needs is inside the compiled prompt and the fact log. This is the contract that makes the system reproducible and auditable.

---

## Changelog Structure

After every document, `belief_changelog.md` records what changed — one entry per affected watch area, appended.

```markdown
## Changelog — {doc_id} — {timestamp}

### Revenue Growth Character
**Action:** [DEEPENED]
**Previous statement:** Revenue growth is price-driven with volume in mild decline.
**New statement:** (unchanged)
**Reason:** Third consecutive document confirms price-volume split. Pattern fingerprint enriched with Q2 language variant.
**Evidence type:** Pattern evidence — structural recurrence
**Maturity impact:** Provisional → Confirmed
**What next document should test:** Whether volume decline steepens past −3% or price contribution begins compressing.

### Cost Absorption Behavior
**Action:** [TENSION]
**Previous statement:** Fixed cost base absorbs volume decline without margin impact above X threshold.
**New statement:** (unchanged — held under tension)
**Reason:** Q2 document shows first margin compression since pattern was established. Single document; not yet sufficient to revise.
**Evidence type:** Contradiction signal
**Maturity impact:** None — held at Confirmed pending next document
**What next document should test:** Whether margin compression continues or Q2 was an isolated event.
```

### Changelog Action Tags

| Tag | Meaning |
|-----|---------|
| `[NEW]` | A watch area added for the first time (Stage: Candidate) |
| `[DEEPENED]` | Statement held; Why Durable, Pattern Fingerprint, or Forward Signal was enriched with new evidence |
| `[UPDATED]` | Statement was revised — new evidence changed the interpretation |
| `[NARROWED]` | Belief scope reduced — the belief now applies more specifically than before |
| `[TENSION]` | A contradicting signal appeared but is not yet strong enough to revise the belief |
| `[SILENCE]` | The document covered this watch area but showed no signal — noted but no update |
| `[RETIRED]` | Belief archived — evidence no longer supports it after repeated absence or strong contradiction |
| `[NO CHANGE]` | Document processed; no update warranted for this watch area |

The changelog is append-only. An absent changelog entry for a document means the pipeline did not complete for that document.

---

## World Model Bounds

- **Max active beliefs per stream:** ~20 (configurable)
- **Decay trigger:** 90 days since last confirming document (configurable)
- **Archive, not delete:** Contradicted and decayed beliefs are archived, not erased. History is preserved.
