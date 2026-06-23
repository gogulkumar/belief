# World Model Schema

The world model (L1) is the living belief model. It is the file an agent loads at session start. It is surgically updated — never rewritten from scratch.

## File Structure

One world model file per active belief type. Example: `world_model/business_memory.md`

```
world_model/
├── business_memory.md
├── business_dynamics.md
├── narrative_understanding.md
├── factual_understanding.md
└── causal_understanding.md
```

## Belief Entry Format

Each belief is one block in its category file. Fields are always present; values update over time.

```markdown
## [BELIEF_ID]

**Statement:** [Durable, falsifiable, actionable interpretation of how the business behaves]

**Confidence:** 0.XX
**Direction:** Improving | Stable | Deteriorating | Unclear
**Stage:** Provisional | Confirmed | Established
**Last Seen:** [Document name or date of most recent confirming document]
**First Seen:** [Document name or date of originating document]

**Normal Baseline:** [The expected or typical state for this dimension — the reference point
that makes deviations meaningful. Not what happened — what is normal.]

**Forward Signal:** [What the next document should show if this belief is holding.
What signal would indicate it is shifting or breaking.]

**Evidence Trail:**
- [Doc 1] — [brief note on what it showed]
- [Doc 2] — [brief note on what it showed]

**Contradictions:** (if any)
- [Doc N] — [brief note on what challenged this belief]
```

The **Stage** field maps to the durability ladder: Provisional (1 document), Confirmed (2), Established (3+). A Provisional belief is explicitly marked as subject to confirmation. An Established belief requires substantially stronger contradicting evidence before revision.

The **Normal Baseline** is what makes anomaly detection possible. Without it, any number is just a number. With it, a deviation is immediately visible and meaningful.

The **Forward Signal** is what makes the belief actionable. It tells the reader exactly what to look for in the next document — and what its absence would mean.

### Example

```markdown
## BM-004

**Statement:** Revenue growth is price-driven with volume in mild decline,
monetizing an existing base rather than expanding it.

**Confidence:** 0.52
**Direction:** Deteriorating
**Last Seen:** HCOM_Business_Review_Q2_2026
**First Seen:** HCOM_Business_Review_Q4_2025

**Evidence Trail:**
- Q4 2025 — Revenue +9% on +11% price, volume −1%. First signal.
- Q1 2026 — Pattern repeats. Volume −2%, price +8%. Confirmed.
- Q2 2026 — Volume now −3%, price holding +7%. Direction tightening.

**Contradictions:**
(none)
```

## Causal Belief Entry Format

Causal beliefs carry additional fields for the lead-lag relationship.

```markdown
## CU-001

**Leading Signal:** [what moves first, with threshold if visible]
**Lagging Outcome:** [what follows]
**Observed Lag:** ~N periods
**Confirmed:** N times
**Status:** Active | Pending | Candidate (Factual Understanding)

**Confidence:** 0.XX  (cap: 0.90)
**Direction:** Improving | Stable | Deteriorating | Unclear

**Evidence Trail:**
- [Doc 1] — leading signal appeared
- [Doc 2] — lagging outcome arrived N periods later

**Contradictions:** (if any)
```

## Update Arithmetic

| Action | Confidence Change |
|--------|------------------|
| Confirm | +0.08 |
| Contradict | −0.15 |
| New Prior | initialized at 0.20 |
| Decay (90+ days unseen) | −0.05 per cycle |

**Bounds:** Floor 0.05 · Cap 0.95 (0.90 for causal beliefs)
**Archive threshold:** Below 0.10 → moved to `world_model/archive/`

## Belief Scope Levels

Beliefs carry an implicit scope level based on how broadly they apply and how rarely they change. Scope level is not a stored field — it is determined by which belief type holds the belief and how it was worded.

| Level | Scope | Change Rate | Typical Belief Type |
|-------|-------|-------------|---------------------|
| **L1 — Global Finance** | Universal accounting and business model mechanics | Rare | Business Memory |
| **L2 — Brand Scope** | Business model rules specific to this brand | Slow (quarters) | Business Memory, Business Dynamics |
| **L3 — Domain Metric** | Tolerance thresholds for specific KPIs | Frequent (each cycle) | Factual Understanding, Causal Understanding |

**Revision thresholds scale with scope level.** A single document that appears to contradict a Level 1 belief is more likely an extraction error than a genuine contradiction — flag it rather than contradicting. A Level 3 belief can be revised on a single strong document.

The scope level also determines how the seeded priors from the Knowledge Dossier behave after seeding. Level 1 beliefs seed at 0.20 and rise slowly; Level 3 beliefs can confirm faster because the evidence arrives in almost every document.

---

## World Model Bounds

- **Max active beliefs per type:** ~20
- **Decay trigger:** 90 days since last confirming document (configurable)
- **Archive, not delete:** Contradicted and decayed beliefs are archived, not erased. History is preserved.

## What the Belief Reasoning Agent Reads

Every Prompt 3 (Belief Reasoning) call receives:

1. **The document chunk** — the text being analyzed right now
2. **The current world model file** for the relevant belief type — all active beliefs
3. **The master memory goal** — the user-defined observation angle from Prompt 1

It writes back **only the lines that change**. The rest of the world model is untouched.

## Changelog Structure

After every ingestion cycle, `belief_changelog.md` records what changed at the belief level — one entry per belief affected.

```markdown
## Changelog — [Document Name] — [Timestamp]

- [NEW] BM-005 — "Cost structure has become predominantly fixed above X volume threshold"
- [UPDATED] BD-002 — Statement revised: evidence from this document changed the interpretation
- [DEEPENED] CU-001 — Statement held; Forward Signal updated with new leading indicator observed
- [RETIRED] NU-003 — Pattern not seen in 3+ consecutive documents; archived
```

| Tag | Meaning |
|-----|---------|
| `[NEW]` | A belief added for the first time (Stage: Provisional) |
| `[UPDATED]` | Statement was revised — new evidence changed the interpretation |
| `[DEEPENED]` | Statement held; Normal Baseline or Forward Signal was enriched |
| `[RETIRED]` | Belief archived — evidence no longer supports it after repeated absence |

The changelog is append-only. It records that a statement changed but not the full before/after diff (open design question — see BELIEF.md §08). An absent changelog entry for a document means that document produced no belief updates across any type.

---

## The Ledger

Every ingestion cycle appends one entry to the ledger. The ledger is immutable and append-only.

```markdown
## Ledger Entry — [Timestamp]

**Document:** [filename]
**Document Type:** [pptx / pdf / transcript / etc]
**Period Covered:** [e.g., Q2 2026]
**Belief Types Run:** [list of active belief types]

**Belief Updates:**
- BM-004: Confirmed (+0.08 → 0.52)
- NU-002: Confirmed (+0.08 → 0.74)
- CU-001: New Prior added (0.20) — "Onboarding decline → churn increase, lag ~1 period"

**Decay Pass:**
- No beliefs archived this cycle

**Source Index Entry Written:** Yes (L2)
**Raw Archive Written:** Yes (L3)
```
